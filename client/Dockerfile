FROM node:14-alpine

WORKDIR /app

COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the client code to the container
COPY . .

# Expose port 3000 
EXPOSE 3000

# Set the command to run the client
CMD ["npm", "start"]
