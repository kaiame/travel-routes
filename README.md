# travel-routes
from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample data for travel routes
routes = [
    {"id": 1, "start": "Paris", "destination": "Rome", "duration": "2h 30m", "transport": "Flight"},
    {"id": 2, "start": "New York", "destination": "Los Angeles", "duration": "6h", "transport": "Flight"},
    {"id": 3, "start": "Berlin", "destination": "Prague", "duration": "4h", "transport": "Train"}
]

@app.route("/routes", methods=["GET"])
def get_routes():
    return jsonify(routes)

@app.route("/route/<int:route_id>", methods=["GET"])
def get_route(route_id):
    route = next((r for r in routes if r["id"] == route_id), None)
    if route:
        return jsonify(route)
    return jsonify({"error": "Route not found"}), 404

@app.route("/add_route", methods=["POST"])
def add_route():
    new_route = request.get_json()
    new_route["id"] = len(routes) + 1
    routes.append(new_route)
    return jsonify(new_route), 201

if __name__ == "__main__":
    app.run(debug=True)
