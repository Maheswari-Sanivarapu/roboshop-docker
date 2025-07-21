Docker layers flow:-
# Dockerfile
FROM ubuntu:20.04                         # Step 1
RUN apt-get update                        # Step 2
RUN apt-get install -y curl              # Step 3
RUN useradd -m appuser                   # Step 4
COPY script.sh /home/appuser/script.sh  # Step 5
RUN chmod +x /home/appuser/script.sh    # Step 6
USER appuser                             # Step 7
CMD ["/home/appuser/script.sh"]         # Step 8


Step 1: FROM ubuntu:20.04
Pulls base image.
Creates Layer 1.
No container is created yet.

Step 2: RUN apt-get update
Docker creates a temporary container from Layer 1.
Runs apt-get update.
Commits the result as Layer 2.
Deletes the container.

Step 3: RUN apt-get install -y curl
Docker creates a new container from Layer 1 + Layer 2.
Installs curl.
Commits as Layer 3.
Deletes the container.

Step 4: RUN useradd -m appuser
New container from Layer 3.
Adds new user.
Commits as Layer 4.
Deletes the container.

Step 5: COPY script.sh /home/appuser/script.sh
New container from Layer 4.
Copies script.sh into the image.
Commits as Layer 5.
Deletes the container.

Step 6: RUN chmod +x /home/appuser/script.sh
New container from Layer 5.
Makes the script executable.
Commits as Layer 6.
Deletes the container.

Step 7: USER appuser
No file system change, only metadata.
Adds Layer 7 with user-switch info.

Step 8: CMD [...]
Adds final metadata layer to define default command.
This is Layer 8.



For Each Dockerfile Instruction:
Docker starts with an image (initially the FROM base image).
Docker creates a temporary container from that image.
Docker executes the instruction inside the container (like RUN, COPY, etc.).
Docker commits the resulting container state as a new image layer.
Docker deletes the temporary container.
The new image (with added layer) becomes the base for the next instruction.

