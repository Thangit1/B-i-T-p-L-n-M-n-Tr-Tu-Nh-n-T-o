# pip install flask
# pip install flask-cors
from unittest import result
from werkzeug.wrappers import Request, Response
from flask import Flask, request, jsonify
from flask import Flask, render_template, request
from flask_cors import CORS, cross_origin
import joblib

# Khởi tạo Flask server backend
app = Flask(__name__, template_folder='templates')

# Apply flask CORS
cors = CORS(app)

# Khai báo route từ index.html

@app.route('/')
# @cross_origin()
def home():
    # Read file index.html and return
    # return open('index.html', 'r', encoding = 'UTF-8').read()
    return render_template('index.html')
    
@app.route('/home', methods=['GET'])
# @cross_origin()
def index():
    # Lấy dữ liệu từ index.html
    no_of_adults = request.args.get('no_of_adults')
    no_of_children = request.args.get('no_of_children')
    no_of_weekend_nights = request.args.get('no_of_weekend_nights')
    no_of_week_nights = request.args.get('no_of_week_nights')
    type_of_meal_plan = request.args.get('type_of_meal_plan')
    required_car_parking_space = request.args.get('required_car_parking_space')
    room_type_reserved = request.args.get('room_type_reserved')
    lead_time = request.args.get('lead_time')
    arrival_year = request.args.get('arrival_year')
    arrival_month = request.args.get('arrival_month')
    arrival_date = request.args.get('arrival_date')
    market_segment_type = request.args.get('market_segment_type')
    repeated_guest = request.args.get('repeated_guest')
    no_of_previous_cancellations = request.args.get('no_of_previous_cancellations')
    no_of_previous_bookings_not_canceled = request.args.get('no_of_previous_bookings_not_canceled')
    avg_price_per_room = request.args.get('avg_price_per_room')
    no_of_special_requests = request.args.get('no_of_special_requests')


    # Tạo dataframe từ dữ liệu
    # df = [[avg_price_per_room, no_of_adults, no_of_children, room_type_reserved, market_segment_type, no_of_special_requests, repeated_guest, no_of_previous_bookings_not_canceled, no_of_previous_cancellations]]
    df = [[no_of_adults, no_of_children, no_of_weekend_nights, no_of_week_nights, type_of_meal_plan, required_car_parking_space, room_type_reserved, lead_time, arrival_year, arrival_month, arrival_date, market_segment_type, repeated_guest, no_of_previous_cancellations, no_of_previous_bookings_not_canceled, avg_price_per_room, no_of_special_requests]]
    print('df = ', df)
    print('df[0] = ', len(df[0]))
    clf = joblib.load('model/model.pkl')
    prediction = clf.predict(df)
    print('kq = ', prediction[0])
    return render_template('index.html', result = prediction[0], no_of_adults = no_of_adults, no_of_children = no_of_children, no_of_weekend_nights = no_of_weekend_nights, no_of_week_nights = no_of_week_nights, type_of_meal_plan = type_of_meal_plan, required_car_parking_space = required_car_parking_space, room_type_reserved = room_type_reserved, lead_time = lead_time, arrival_year = arrival_year, arrival_month = arrival_month, arrival_date = arrival_date, market_segment_type = market_segment_type, repeated_guest = repeated_guest, no_of_previous_cancellations = no_of_previous_cancellations, no_of_previous_bookings_not_canceled = no_of_previous_bookings_not_canceled, avg_price_per_room = avg_price_per_room, no_of_special_requests = no_of_special_requests)

# Start Backend server
if __name__ == '__main__':
    # clf = joblib.load('model.pkl')
    # app.run(host='0.0.0.0', port='9000', debug=True)
    from werkzeug.serving import run_simple
    run_simple('localhost', 8999, app)
    
