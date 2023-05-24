# laba08
Исходный код серверной части (server.py):

```
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/api/data')
def get_data():
    data = {'message': 'Hello World!'}
    return jsonify(data)

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

Dockerfile для серверной части:

```
FROM python:3.8-slim-buster

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY server.py .

EXPOSE 5000

CMD ["python", "server.py"]
```

requirements.txt для серверной части:

```
flask
```

Исходный код клиентской части (client.html):

```
<!DOCTYPE html>
<html>
<head>
    <title>Client</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <div id="data"></div>
    <script>
        $.getJSON('/api/data', function(data) {
            $('#data').html(data.message);
        });
    </script>
</body>
</html>
```

Dockerfile для клиентской части:

```
FROM nginx:1.21.3-alpine

COPY client.html /usr/share/nginx/html/index.html
```

docker-compose.yml:

```
version: '3'

services:
  server:
    build: ./server
    ports:
      - 5000:5000
    networks:
      - my-network

  client:
    build: ./client
    ports:
      - 80:80
    networks:
      - my-network

networks:
  my-network:
    driver: bridge
```

Сборка и запуск:

```
$ docker-compose build
$ docker-compose up -d
```
![изображение](https://github.com/Wenir04/laba08/assets/113133600/625b4aee-90af-42f2-bbd3-1f749aaff68e)

