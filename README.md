# Setup

## Docker

Make sure Docker and Docker Compose are installed on the instance.

1. Go to the directory containing both front-end and back-end directories
3. Make sure the following files are present:
   - .env (generate keys using `openssl rand -base64 64`)
   - .env.sample
   - docker-compose.yml
   - This README
4. Run in a screen

```bash
docker compose build
docker compose up
```

5. When finished, run

```bash
docker compose down
```

## Create an admin user

While the containers are running run the following commands:

1. Generate a password hash

```bash
docker exec leaveboard-server node -e "const bcrypt = require('bcryptjs'); console.log('New hash:', bcrypt.hashSync('{{PLAIN_TEXT_PASSWORD}}', 10));"
```

2. Copy the output value, this will be the `ADMIN_PASSWORD_HASH`
3. Create an admin user:

```bash
# Connect to the container shell
docker exec -it mongo mongosh

# Switch to the leaveboard database
use leaveboard

# Check if a usr exists
db.user.find()

# Create an admin user
db.user.insertOne({
    name: "New Admin",
    email: "{{ADMIN_EMAIL}}",
    password: "{{ADMIN_PASSWORD_HASH}}",
    role: "admin",
    position: "Administrator",
    team: "Management",
    office: "HQ",
    country: "US",
    wfhWeekly: 1,
    leaveCounts: { sickLeave: 15, timeOff: 15 },
    employmentDate: new Date(),
    isActive: true,
    createdAt: new Date(),
    updatedAt: new Date()
})
```

## Development

See the `volume` and `command` lines in the `docker-compose.yml` file. During development, uncomment these lines in order to override the Dockerfiles' `CMD` instructions and enable hot reloading.
