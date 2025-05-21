// ...existing code...
import React, { useState } from "react";
import { Line } from "react-chartjs-2";
import {
  Card,
  CardContent,
} from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Plus, Trash } from "lucide-react";
import "chart.js/auto";

export default function App() {
  const [trips, setTrips] = useState([]);
  const [form, setForm] = useState({
    driver: "",
    vehicleId: "",
    destinations: [""],
    startMileage: "",
    endMileage: "",
    fuelUsed: "",
    issues: "",
    date: "",
  });

  const handleChange = (e, index) => {
    if (e.target.name === "destination") {
      const updated = [...form.destinations];
      updated[index] = e.target.value;
      setForm({ ...form, destinations: updated });
    } else {
      setForm({ ...form, [e.target.name]: e.target.value });
    }
  };

  const addDestination = () =>
    setForm({ ...form, destinations: [...form.destinations, ""] });

  const removeDestination = (index) => {
    const updated = form.destinations.filter((_, i) => i !== index);
    setForm({ ...form, destinations: updated });
  };

  const handleSubmit = () => {
    const distance = Number(form.endMileage) - Number(form.startMileage);
    setTrips([...trips, { ...form, distance }]);
    setForm({
      driver: "",
      vehicleId: "",
      destinations: [""],
      startMileage: "",
      endMileage: "",
      fuelUsed: "",
      issues: "",
      date: "",
    });
  };

  const chartData = {
    labels: trips.map((trip) => trip.date),
    datasets: [
      {
        label: "Distance Travelled (km)",
        data: trips.map((trip) => trip.distance),
        fill: false,
        borderColor: "rgb(59,130,246)",
        tension: 0.2,
      },
    ],
  };

  return (
    <div className="p-6 max-w-6xl mx-auto space-y-6">
      <Card className="bg-white shadow-md p-4">
        <CardContent className="space-y-4">
          <h2 className="text-xl font-bold">Log New Trip</h2>

          <div className="grid grid-cols-2 gap-4">
            <Input
              placeholder="Driver Name"
              name="driver"
              value={form.driver}
              onChange={handleChange}
            />
            <Input
              placeholder="Vehicle ID"
              name="vehicleId"
              value={form.vehicleId}
              onChange={handleChange}
            />
            <Input
              placeholder="Start Mileage (km)"
              name="startMileage"
              type="number"
              value={form.startMileage}
              onChange={handleChange}
            />
            <Input
              placeholder="End Mileage (km)"
              name="endMileage"
              type="number"
              value={form.endMileage}
              onChange={handleChange}
            />
            <Input
              placeholder="Fuel Used (litres)"
              name="fuelUsed"
              type="number"
              value={form.fuelUsed}
              onChange={handleChange}
            />
            <Input
              type="date"
              name="date"
              value={form.date}
              onChange={handleChange}
            />
          </div>

          <div className="space-y-2">
            <label className="font-semibold">Destinations</label>
            {form.destinations.map((dest, index) => (
              <div key={index} className="flex items-center gap-2">
                <Input
                  placeholder={`Destination ${index + 1}`}
                  value={dest}
                  name="destination"
                  onChange={(e) => handleChange(e, index)}
                />
                {index > 0 && (
                  <Button
                    variant="destructive"
                    onClick={() => removeDestination(index)}
                  >
                    <Trash size={16} />
                  </Button>
                )}
              </div>
            ))}
            <Button variant="secondary" onClick={addDestination}>
              <Plus className="mr-2" size={16} /> Add Destination
            </Button>
          </div>

          <div>
            <label className="font-semibold">Vehicle Issues (optional)</label>
            <Textarea
              placeholder="Describe any issues encountered..."
              name="issues"
              value={form.issues}
              onChange={handleChange}
            />
          </div>

          <Button className="w-full mt-4" onClick={handleSubmit}>
            Save Trip
          </Button>
        </CardContent>
      </Card>

      <Card>
        <CardContent className="py-6">
          <h2 className="text-xl font-bold mb-4">Trip Distance Chart</h2>
          <Line data={chartData} />
        </CardContent>
      </Card>

      <Card>
        <CardContent className="overflow-x-auto py-6">
          <h2 className="text-xl font-bold mb-4">Trip History</h2>
          <table className="min-w-full border">
            <thead className="bg-gray-100">
              <tr>
                <th className="border p-2">Driver</th>
                <th className="border p-2">Vehicle ID</th>
                <th className="border p-2">Destinations</th>
                <th className="border p-2">Start</th>
                <th className="border p-2">End</th>
                <th className="border p-2">Distance</th>
                <th className="border p-2">Fuel</th>
                <th className="border p-2">Date</th>
                <th className="border p-2">Issues</th>
              </tr>
            </thead>
            <tbody>
              {trips.map((trip, idx) => (
                <tr key={idx} className="text-sm">
                  <td className="border p-2">{trip.driver}</td>
                  <td className="border p-2">{trip.vehicleId}</td>
                  <td className="border p-2">
                    {trip.destinations.join(", ")}
                  </td>
                  <td className="border p-2">{trip.startMileage}</td>
                  <td className="border p-2">{trip.endMileage}</td>
                  <td className="border p-2">{trip.distance} km</td>
                  <td className="border p-2">{trip.fuelUsed}L</td>
                  <td className="border p-2">{trip.date}</td>
                  <td className="border p-2">{trip.issues || "N/A"}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </CardContent>
      </Card>
    </div>
  );
}
// ...existing code...
