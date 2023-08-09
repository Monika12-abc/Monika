from flask import Flask, jsonify
import datetime

app = Flask(__name__)

# Sample train data (replace this with your actual data source)
# Each train is represented as a dictionary with 'departure_time', 'seats_available', and 'price' attributes.
# The departure_time is expected to be a datetime object.
trains = [
    {
        'departure_time': datetime.datetime(2023, 8, 9, 10, 0),
        'seats_available': 50,
        'price': 30.0
    },
    {
        'departure_time': datetime.datetime(2023, 8, 9, 12, 30),
        'seats_available': 20,
        'price': 40.0
    },
    # Add more train entries here...
]

@app.route('/trains', methods=['GET'])
def get_trains():
    current_time = datetime.datetime.now()
    twelve_hours_later = current_time + datetime.timedelta(hours=12)
    
    upcoming_trains = []
    for train in trains:
        if current_time <= train['departure_time'] <= twelve_hours_later:
            upcoming_trains.append({
                'departure_time': train['departure_time'].strftime('%Y-%m-%d %H:%M:%S'),
                'seats_available': train['seats_available'],
                'price': train['price']
            })
    
    return jsonify(upcoming_trains)

if __name__ == '__main__':
    app.run(debug=True)
