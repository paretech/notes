# Tandoor Recipes

[**Tandoor Recipes**][tan1] (herein simply "Tandoor") is a web based recipe manager written on Python Django. 

## Resources
- [Tandoor Recipes - Documentation][tan1]
- [Tandoor Recipes - GitHub][tan2]
- [Tandoor Recipes - Docker Install Instructions][tan3]
- [Tandoor Recipes - Docker Hub][tan4]
- [Docker Hub - Quickstart][dcr1]
- [Docker Compose - Get started with Docker Compose][dcr2]

[tan1]: https://docs.tandoor.dev/ "Tandoor Recipes - Documentation"
[tan2]: https://github.com/TandoorRecipes/recipes "Tandoor Recipes - GitHub"
[tan3]: https://docs.tandoor.dev/install/docker/ "Tandoor Recipes - Docker Install Instructions"
[tan4]: https://hub.docker.com/r/vabene1111/recipes/tags "Tandoor Recipes - Docker Hub"
[dcr1]: https://docs.docker.com/docker-hub/ "Docker Hub - Quickstart"
[dcr2]: https://docs.docker.com/compose/gettingstarted/ "Docker Compose - Get started with Docker Compose"

## Installation Notes
- [Docker is the recommended install method for Tandoor][tan3].
  - There is currently no way to migrate back to an older version of Tandoor.
  - Tandoor recommends Docker Compose method of install unless you have a reason not to. 
    - Specify SECRET_KEY and POSTGRES_PASSWORD
    - Tandoor documentation recommends using "Latest" image from [Docker Hub][tan4] but I opted to use the 1.2.7 since the versions appear to update frequently (every 3-10 days) and there is no way to downgrade. I'm trying to get the most stability as possible.
    - At time of writing, "Latest" is likely 1.2.7 since they have the same publish date, but Latest doesn't excplicitely state that.


### High Level Install Steps
  1. Pick install method [Docker]
  1. Pick Docker Hub image recipe [1.2.7]
     - `docker pull vabene1111/recipes:1.2.7`
  1. Create directory for managing Docker Config. Presently using `~\repos\tandoor` but may need to consider other locations for configuration management (like using a git repo).
     - `mkdir ~\source\repos\tandoor`
  1. Pick Docker Compose Install Configuration [Plain]
     - `wsl wget https://raw.githubusercontent.com/vabene1111/recipes/develop/docs/install/docker/plain/docker-compose.yml`
  1. Revise Docker .env files
     - Specify SECRET_KEY and POSTGRES_PASSWORD. You can pick anything for these values as they are used to configure the other services. If you are deploying this, these parameters need more attention.
  1. Start container
     - `docker-compose up -d`

If you try to start without updating .env file, you will note that the Docker Desktop app indicates that the postgres database service keeps restarting.


### Install Configuration
- Windows 10
- Docker 4.8.2 (79419)
- Docker Compose
- Tandoor 1.2.7 (fixed)

## Additional Reading
- https://stackoverflow.com/questions/44784103/where-should-i-put-docker-compose-yml
- https://nickjanetakis.com/blog/docker-tip-76-where-to-put-docker-compose-projects-on-a-server
- https://hay-kot.github.io/mealie/ 
