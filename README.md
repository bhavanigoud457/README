# README
For the Log Ingestor:

python
from flask import Flask, request, jsonify

app = Flask(__name__)

logs = []

@app.route('/ingest', methods=['POST'])
def ingest_log():
    new_log = request.get_json()
    logs.append(new_log)
    return jsonify({'message': 'Log ingested successfully'}), 201

if __name__ == '__main__':
    app.run(port=3000)


This is a simple Flask app that listens for POST requests at the `/ingest` endpoint, expecting JSON logs in the request body.

For the Query Interface:

python
from flask import Flask, request, jsonify

app = Flask(__name__)

logs = []

# Assume logs are ingested into this list from the Log Ingestor

@app.route('/search', methods=['GET'])
def search_logs():
    query_params = request.args.to_dict()
    filtered_logs = filter_logs(logs, query_params)
    return jsonify({'logs': filtered_logs})

def filter_logs(logs, filters):
    filtered_logs = logs.copy()
    for key, value in filters.items():
        filtered_logs = [log for log in filtered_logs if log.get(key) == value]
    return filtered_logs

if __name__ == '__main__':
    app.run(port=5000)  # You can choose a different port for the query interface


This example assumes that the logs are ingested into the `logs` list. The `search_logs` function filters logs based on query parameters.

Remember to install Flask (`pip install Flask`) if you haven't already. This is a basic structure; you'll need to add more features and integrate a database for scalability.

