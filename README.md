# Setup

## Docker

1. Go to the directory containing both front-end and back-end directories
2. Make sure Docker is installed
3. Make sure the following files are present:
   - .env
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

1. Generate a password hash

```bash
docker exec leaveboard-server node -e "const bcrypt = require('bcryptjs'); console.log('New hash:', bcrypt.hashSync('admin123', 10));"
```

2. Copy the output value
3. Create an admin user
```
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
