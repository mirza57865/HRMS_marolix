With Docker:
sudo apt install -y docker.io docker-compose
git clone https://github.com/Nagendra206/HRMS_marolix.git
sudo systemctl start docker
sudo usermod -aG docker ubuntu
sudo systemctl enable docker
cd HRMS_marolix/HRMS-Server
vi Dockerfile
 
 FROM python:3.8
 ENV PYTHONDONTWRITEBYTECODE 1
 ENV PYTHONUNBUFFERED 1
 WORKDIR /app
 COPY . /app/
 RUN pip install -r requirements.txt
 EXPOSE 8000
 CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
vi docker-compose.yml
 
 version: '3'
 services:
 db:
 image: postgres:12
 environment:
 POSTGRES_DB: hrms_marolix
 POSTGRES_USER: marolix
 POSTGRES_PASSWORD: password
 web:
 build: .
 command: python manage.py runserver 0.0.0.0:8000
 volumes:
 - .:/app
 ports:
 - "8000:8000"
 depends_on:
 - db
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql.service
sudo -i -u postgres
 psql
 CREATE DATABASE hrms_marolix;
 CREATE USER marolix with encrypted password 'password';
 GRANT ALL PRIVILEGES ON DATABASE hrms_marolix TO marolix;
cd mHRMS/
ls
vi settings.py 
cd ..
docker-compose up --build -d
docker image ls
docker ps
docker logs hrms-server_web_1
cd mHRMS/
vi settings.py 
 Replace 'localhost' with 'db'
docker-compose restart web
