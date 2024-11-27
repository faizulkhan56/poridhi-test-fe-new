# poridhi-test-fe-new
Setting Up the Frontend (React)
2.1. Create a Directory for the Frontend
cd ..
npx create-react-app frontend

2.2. Modify the React App
Navigate to frontend/src/App.js and replace the content with the following code:
import React, { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState('Loading...');

  const callAPI = async () => {
    try {
      const res = await fetch("http://localhost:4000/");
      const data = await res.json();
      setData(data.message);
    } catch (error) {
      console.log('Error:', error);
    }
  };

  useEffect(() => {
    callAPI();
  }, []);

  return (
    <div className="App">
      <h1>Backend Response:</h1>
      <p>{data}</p>
    </div>
  );
}

export default App;

This fetches the message from the backend and displays it on the frontend.
2.3. Create a Dockerfile for the Frontend
In the frontend directory, create a Dockerfile:
touch Dockerfile

Add the following content:
# Stage 1: Build the React app
FROM node:16-alpine AS build

WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the code
COPY . .

# Build the React app for production
RUN npm run build

# Stage 2: Serve the React app
FROM nginx:alpine

# Copy the built app to the NGINX container
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 to access the React app
EXPOSE 80

# Start NGINX
CMD ["nginx", "-g", "daemon off;"]

This Dockerfile uses a multi-stage build. The first stage builds the React app, and the second stage uses Nginx to serve the static files.
Step 3: Writing the Makefile
Let's create a Makefile to build and push the Docker images.
Create the Makefile
In the root directory, create a Makefile:
touch Makefile

How to Use:
First Login into Docker hub
docker login


Provide the credentials as needed.

To build, tag, and push the frontend image:
make all-frontend


To build, tag, and push the backend image:
make all-backend


To build, tag, and push both images in parallel:
make all



Check the created images
After completion of the make command, you can check if docker images:
docker images


Step 4: Running the Application
Now that we have Docker images for both the frontend and backend, let's run the entire application using Docker.
4.1. Run the Backend
Run the backend image:
docker run -p 4000:4000 your-dockerhub-username/<backend-image-name>


This will run the backend server on port 4000.
4.2. Run the Frontend
Run the frontend image:
docker run -p 80:80 your-dockerhub-username/<frontend-image-name>

This will run the frontend on port 80.


