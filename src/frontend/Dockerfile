# Use base node 8 image from Docker hub
FROM node:8

WORKDIR /frontend

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy rest of the application csource code
COPY . .

# Run app.js with debugging port when container launches
# ENTRYPOINT ["node", "--inspect=9229", "app.js"]

USER 0
# Set permissions on /etc/passwd and /home to allow arbitrary users to write
RUN mkdir -p /home/user && chgrp -R 0 /home && chmod -R g=u /etc/passwd /home

RUN echo "PS1='\s-\v \w \$ '" > "${HOME}"/.bashrc

USER 10001

ENV HOME=/home/user
WORKDIR /projects

# Comment above and uncomment below to run app.js without debugger port when container launches
ENTRYPOINT ["node", "app.js"]