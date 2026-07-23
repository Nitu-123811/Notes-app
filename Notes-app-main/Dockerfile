FROM node:20 AS frontend

WORKDIR /app/frontend

COPY mynotes/package*.json ./

RUN npm install

COPY mynotes .

RUN npm run build


FROM python:3.9

WORKDIR /app/backend

COPY requirements.txt .

RUN apt-get update \
    && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
    && rm -rf /var/lib/apt/lists/*

RUN pip install mysqlclient
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

COPY --from=frontend /app/frontend/build ./mynotes/build

EXPOSE 8000

CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
