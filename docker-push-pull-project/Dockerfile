# Use a lightweight official Node (Base) image
FROM node:18-alpine

# From now on, every command that follows will run inside the directory /app inside the container's filesystem.
# So inside your container, there will be a directory /app that becomes your project’s working folder.
WORKDIR /app

# copy package files and install production deps only to the /app directory inside container's filesystem
COPY package*.json ./
RUN npm ci --production

# copy app files, Docker builds images in layers. Each COPY (the previous copy -> package*.json), RUN, etc. creates a new layer that gets cached if nothing changes.
# If you copied everything (COPY . .) before installing,
# even changing one line in your index.js would break the cache → Docker would reinstall dependencies every build.
COPY . .



# It tells Docker (and others) that the container is expected to listen on port 8080. It's more like a label that says “Hey, I'm using 8080 inside this container.
# When you run the container: docker run -p 3000:8080 my-node-app, So the mapping means: "Hey Docker, if someone connects to my computer’s port 3000, please forward that traffic to port 8080 inside the container."
# The host is your actual machine - your laptop, desktop, or cloud VM where Docker Engine is installed. A network port on your host OS - e.g., macOS, Windows, or Linux.
EXPOSE 8080
ENV NODE_ENV=production

CMD ["node", "index.js"]
