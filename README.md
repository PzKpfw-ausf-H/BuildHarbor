# BuildHarbor
## Itch io equivalent (or kind of)
Build Harbor is a platform where a **developer** can:

1. register and log in
2. create a games page
3. create a games version
4. create and upload a game build (for unity as example)
5. upload a ZIP into an S3-compatible storage
6. start an async build processing
7. get the processing result
8. publish the game
9. provide a ready-to-download game build

A **standart user** can:

1. open the games catalog
2. sort the games
3. open the games page
4. choose the platform and a version
5. download the game build
   
## MVP limitations
Taking into account that this is a **pet** project, BuildHarbor is going to have these limitations.  
BuildHarbor:
- does NOT run the uploaded files
- does NOT scan for the viruses
- does NOT unpack the archives into the public directory
- does NOT support payment and any social functions
- does NOT guarantees the safety of the game after it is downloaded  
  
The worker checks the ZIP's structure but doesn't confirm the safety of the executable file

## Main scenarios
### Scenario A. Game creation
> Developer
> - creates the profile
> - creates draft-game
> - redacts the description
> - uploads screenshots  
> The game is still not public

### Scenario B. Uploading the build
> Developer
> - creates the game version
> - chooses the platform
> - creates a build
> - uploads ZIP into the object storage
> - confirms the upload  
> after that build.uploaded event is being created  

### Scenario C. Build processing
> Worker
> - gets build.uploaded
> - reads ZIP
> - checks the limitations and the structure
> - calculates the checksum/metadata
> - publishes the result

### Scenario D. Game publishing
> The game can be uploaded **only if**:
> - all the required field are not empty
> - has cover
> - has at least one version
> - has at least one build with *ready* status
> - version and build both belong to **this** game
> - the user is the owner of the game

### Scenario E. Downloading the game
> User
> - opens any published game
> - chooses the platform and the version
> - clicks download

## Tech Stack
- Go
- chi
- PostgreSQL
- pgx
- goose
- MinIO
- RabbitMQ
- Docker Compose
- OpenAPI
- Github Actions
- slog
