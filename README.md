# mesh-directus-replication
A temporary project to replicate the errors when connection to Directus

### Setup
1. Run `yarn install`
1. Set `ADMIN_EMAIL` and `ADMIN_PASSWORD` in `docker-compose.yml`
    - This will be your login to Directus
1. Run `docker-compose up`
1. Navigate to Directus at `http://localhost:8055` and login
1. Click the user icon in bottom left corner.
1. Scroll to bottom and set "Token" (Click check mark at top to save)
1. Open `.meshrc.yaml` and put the "Token" value you chose after "Bearer"
    - e.g. `Authorization: Bearer ThisIsMyToken`
1. Navigate back to Directus and click the "Gear" icon, followed by "Data Model" in left hand menu.
1. Create 3 data models in total.
    - e.g. `lesson`, `video`, `article`
    - Don't worry about selecting anything, just click through after naming table
1. For each data model, click "Create Field" and add a "Standard field" (just provide a name and click through).
1. Create a "Many to Any" field on `lesson` data model named `steps`, with allowed relations of `video` and `article`.
1. Click the "Box" icon in left hand menu and create a record for every data model (collection)
1. For the course record, add the lesson and article record as steps.
1. Run `yarn graphql-mesh serve`
1. Query for 
```
{
  items {
    lesson {
      steps {
        item {
          ... on video {
            title
          }
          ... on article {
            title
          }
        }
      }
    }
  }
}
```
1. Uncomment each transform one at a time in `.meshrc.yaml` and re-run the above query (with necessary adjustments) to observe the errors.