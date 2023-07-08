FROM node:14-alpine

WORKDIR /app

COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the backend code to the container
COPY . .

# Set the command to run the client
CMD ["npm", "start"]