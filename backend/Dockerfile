FROM node:14-alpine

WORKDIR /app/backend

COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the backend code to the container
COPY . .

EXPOSE 5000

# Set the command to run the client
CMD ["npm", "run", "start"]
