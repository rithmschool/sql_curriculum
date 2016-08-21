# Introduction

### Installing Postgres

```
brew install postgres --no-ossp-uuid
brew pin postgres

# Initialize db if none exists already
initdb /usr/local/var/postgres

# Create launchctl script
mkdir -p ~/Library/LaunchAgents
cp /usr/local/Cellar/postgresql/VERSION/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/

# Edit launchctl script (set to not start automatically and keepalive false)
subl ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Inject launchctl script
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Start PostgreSQL
start pg
```

### Definitions

Relatonal Database - 

SQL -

RDBMS - 

Postgres - 

### SQL Syntax

`\du` - lists users
`\dt` - lists tables
`\d+ table_name` - list details about the table name
`\l` - lists databases