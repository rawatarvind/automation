How to write script in python to use psutil and flask.

Prequisitive
1. OS linux ( CentOS and  Ubuntu, etc)
2. python3 should be installed.
3. Docker should  be installed.


let us start to write script.

i am using ubuntu 22.04 you can choose your own OS.

# mkdir  cloudnative

# cd cloudnative
# mkdir templates

create file index.html inside the templates directory.

render_flask is use for clourfull content 

# vim app.py
import psutil
from flask import Flask, render_template

app = Flask(__name__)
		
@app.route("/")
def index():
    cpu_metric = psutil.cpu_percent()
    mem_metric = psutil.virtual_memory().percent
    message =   None
    if cpu_metric > 80 or mem_metric > 80:
        Message = "High CPU or Memory Detect scale up"
    return render_template("index.html",cpu_metric=cpu_metric, mem_metric=mem_metric, message=message)

if __name__=="__main__":
    app.run(debug=True, host= '0.0.0.0')


then run on locally on your terminal.

# python3 app.py

# 
Make  Docker Image from Python Code.

you will be needed  some packages in image or container.

create a file some working directory.

# tocuh requirements.txt

# vim requirements.txt

Flask==2.2.3
MarkupSafe==2.1.2
Werkzeug==2.2.3
itsdangerous==2.1.2
psutil==5.8.0
plotly==5.5.0
tenacity==8.0.1
boto3==1.9.148
kubernetes==10

now create  Docker file 

# vim Dockerfile

FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .

RUN pip3 install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["flask","run"]



save and exit



now the image from Dockerfile 

# docker build -t monitor  .

# docker images

# docker login 

username: 
password:

# docker push ashurawat123/monitor:latest


now create a container from docker image

# docker run -itd --name monitor -p 5000:5000 monitor:latest

# docker ps 

# curl http;//localhost:5000


