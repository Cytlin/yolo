FROM node:14-alpine

WORKDIR /app/client

COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the client code to the container
COPY . .

# Build the production version of the application
RUN npm run build

# Expose port 3000
EXPOSE 3000

# Serve the application using a static web server
CMD ["npx", "serve", "-s", "build", "-l", "3000"]
























# FROM node:14-alpine

# WORKDIR /app/backend

# COPY package*.json ./

# # Install dependencies
# RUN npm install

# # Copy the backend code to the container
# COPY . .

# EXPOSE 5000

# # Set the command to run the client
# CMD ["npm", "run", "start"]
