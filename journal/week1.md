# Week 1 — App Containerization

## Week 1 Summary

In Week one, I will be containerizing crudder app: 
1. I will first make sure that gitpod is connected to my github to leverage on gitpod's dev-as-service properties 
2. After that, I will make sure that my backend is also containerized with docker
3. I will containerize the frontend with docker 
4. Next, I will use docker compose to manage multiple conatiner ie; the frontend and the backed end
5. Finally, I will Integrate app with a database


## Connecting Gitpod to Github:

### Reference: [Link to reference](https://www.gitpod.io/docs/configure/authentication/github)
1. I logged onto my Github repository and locted my Cruddur repo
2. Infront of my repo URL I inserted this ```gitpod.io/``` connecting my Github to Gitpod
3. I clicked on gipod to locate my workspace to see everything is fine in there
4. The entire Crudder app repo was there just like Github

![Link to Github and Gitpod connection](assets/gitpod-github.png)


## Containerzing the Backtend of Crudder:

NB: I added the extension for docker in Gitpod first.
After that, we will install flask which is used for developing web applications using python. Advantages of using Flask framework are: There is a built-in development server and a fast debugger provided [More information on Flask](https://www.analyticsvidhya.com/blog/2021/10/easy-introduction-to-flask-framework-for-beginners/)


### Create a docker file for the backend and run it locally

After that install th python libraries for your backend ```pip3 install -r requirements.txt```
Set your environment variables, with this app we set 2 variables for backend and frontend with this 
```export FRONTEND_URL="*"``` and ```export BACKEND_URL="*"```

After installing your evironment variables, you install your backend app framework with this, in our case is flask 
``` python3 -m flask run --host=0.0.0.0 --port=4567 ```
check the ports and open it ie; the padlock and then open to see it, your app points to ```/api/activities/home``` so append it to the URL before it works

Now you can kill your process ie; stop running your app with this ```Ctrl + c ```

I used this command to install flask which will run on port 4567, at this point is necessary to note which port your container will be running on, it helps to identify possible errors when running multiple containers
in summary for the above below is the compiled code

```
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```

### With this knowledge, we will then run this app in a container for the backend

locate the backend and create a docker file by name ```dockerfile``` 
The dockerfile allows us to run our containers, in other words, an image would be generated from this file to run our container
Make sure to be in the backend and then paste your docker file command, in our case is this

```
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"] ```




