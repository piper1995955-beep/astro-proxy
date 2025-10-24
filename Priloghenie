from flask import Flask, request, jsonify

app = Flask(__name__)

@app.get("/healthz")
def healthz():
    return {"ok": True}

# /ephem/natal?date_local=YYYY-MM-DDTHH:MM&tz_name=Europe/Moscow&lat=55.75&lon=37.62
@app.get("/ephem/natal")
def natal():
    date = request.args.get("date_local", type=str)
    tz = request.args.get("tz_name", type=str)
    lat = request.args.get("lat", type=float)
    lon = request.args.get("lon", type=float)

    if not date or not tz or lat is None or lon is None:
        return jsonify({"error": "missing required params: date_local, tz_name, lat, lon"}), 400

    return jsonify({
        "meta": {"source": "remote", "tz": tz, "system": "Placidus", "zodiac": "tropical"},
        "datetime": f"{date}Z",
        "location": {"lat": lat, "lon": lon},
        "angles": {"ASC": None, "MC": None, "IC": None, "DSC": None},
        "houses": {},
        "planets": {
            "Sun": {"lon": 123.4, "speed": 0.98, "retro": False},
            "Moon": {"lon": 210.2, "speed": 13.2, "retro": False},
            "Mercury": {"lon": 95.1, "speed": 1.2, "retro": False},
            "Venus": {"lon": 10.3, "speed": 1.2, "retro": False},
            "Mars": {"lon": 350.9, "speed": 0.6, "retro": False},
            "Jupiter": {"lon": 15.4, "speed": 0.08, "retro": False},
            "Saturn": {"lon": 320.1, "speed": 0.03, "retro": True},
        },
    })

# /ephem/transits?natal_json=%7B...URLENCODED...%7D&start=YYYY-MM-DD&end=YYYY-MM-DD
@app.get("/ephem/transits")
def transits():
    natal_json = request.args.get("natal_json", type=str)
    start = request.args.get("start", type=str)
    end = request.args.get("end", type=str)

    if not natal_json or not start or not end:
        return jsonify({"error": "missing required params: natal_json, start, end"}), 400

    return jsonify({
        "meta": {"source": "remote", "system": "Placidus", "zodiac": "tropical"},
        "range": {"start": start, "end": end},
        "events": []
    })
