import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";

export default function SnackStore() {
  const [snacks, setSnacks] = useState([
    { id: 1, name: "Maggi", price: 40, stock: 10, image: "" },
    { id: 2, name: "Chips", price: 20, stock: 15, image: "" },
    { id: 3, name: "Cold Drink", price: 30, stock: 8, image: "" },
    { id: 4, name: "Sandwich", price: 50, stock: 5, image: "" },
    { id: 5, name: "Chocolate", price: 25, stock: 20, image: "" },
  ]);

  const [isAdmin, setIsAdmin] = useState(false);
  const [password, setPassword] = useState("");

  const handleLogin = () => {
    if (password === "admin123") {
      setIsAdmin(true);
      setPassword("");
    } else {
      alert("Incorrect password");
    }
  };

  const handleLogout = () => {
    setIsAdmin(false);
  };

  const addSnack = () => {
    setSnacks([...snacks, { id: Date.now(), name: "New Snack", price: 0, stock: 0, image: "" }]);
  };

  const updateSnack = (id, field, value) => {
    setSnacks(
      snacks.map((snack) =>
        snack.id === id ? { ...snack, [field]: value } : snack
      )
    );
  };

  const deleteSnack = (id) => {
    setSnacks(snacks.filter((snack) => snack.id !== id));
  };

  return (
    <div className="p-6 max-w-3xl mx-auto">
      <h1 className="text-2xl font-bold mb-4">🍕 Late Night Snacks Menu</h1>

      {/* Customer View */}
      {!isAdmin && (
        <div>
          <div className="grid grid-cols-2 gap-4 mb-6">
            {snacks.map((snack) => (
              <Card key={snack.id}>
                <CardContent className="flex flex-col items-center p-4">
                  {snack.image && (
                    <img src={snack.image} alt={snack.name} className="w-24 h-24 object-cover mb-2" />
                  )}
                  <p className="font-semibold">{snack.name}</p>
                  <p>₹{snack.price}</p>
                  <p className="text-red-600">Items left: {snack.stock}</p>
                </CardContent>
              </Card>
            ))}
          </div>

          <div className="flex gap-2 mt-6">
            <Input
              type="password"
              placeholder="Admin Password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
            />
            <Button onClick={handleLogin}>Login</Button>
          </div>
        </div>
      )}

      {/* Admin View */}
      {isAdmin && (
        <div>
          <div className="flex justify-between items-center mb-4">
            <h2 className="text-xl font-semibold">Admin Portal</h2>
            <Button variant="secondary" onClick={handleLogout}>Logout</Button>
          </div>

          <Button className="mb-4" onClick={addSnack}>Add Snack</Button>

          <div className="grid grid-cols-1 gap-4">
            {snacks.map((snack) => (
              <Card key={snack.id}>
                <CardContent className="flex flex-col gap-2 p-4">
                  <Input
                    value={snack.name}
                    onChange={(e) => updateSnack(snack.id, "name", e.target.value)}
                  />
                  <Input
                    type="number"
                    value={snack.price}
                    onChange={(e) => updateSnack(snack.id, "price", Number(e.target.value))}
                  />
                  <Input
                    type="number"
                    value={snack.stock}
                    onChange={(e) => updateSnack(snack.id, "stock", Number(e.target.value))}
                    placeholder="Stock"
                  />
                  <Input
                    type="text"
                    value={snack.image}
                    onChange={(e) => updateSnack(snack.id, "image", e.target.value)}
                    placeholder="Image URL"
                  />
                  <Button variant="destructive" onClick={() => deleteSnack(snack.id)}>
                    Delete
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
