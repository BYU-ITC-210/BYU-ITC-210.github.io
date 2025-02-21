---
Mongo DB Atlas Setup
---
**MongoDB** is a "NoSQL" document database that we will use in this lab. MongoDB **Atlas** is a cloud-hosted MongoDB service that is free for development purposes. Using a cloud service avoids having to install a database server and lets you share the same database between your local development instance and your AWS hosted instance.

In Atlas, we will set up a new database, create a new collection, add some data, and finally create a user that has access to it to be able to read and write data to the database.

1. Browse to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Click **Try Free** to create an account. You can use a Google account but I prefer using my own email address and password.
3. If you created an account with an email address, it will ask you to check your email and click the link to verify.
4. They ask you some survey questions. Those are just for their research.
5. The **Deploy your cluster** page appears.
    - A cluster is a set of servers that hosts your database. To handle the extreme scale that MongoDB is capable of, a single database can be deployed across multiple services. Conveniently, Atlas manages all of that for you.
    - Select the **Free** plan.
    - Give your cluster a name or accept the "Cluster0" default.
    - Select whichever provider and region make sense to you. Of course, you should choose a region based in the U.S. for performance reasons.
    - No need to create any tags.
    - Click **Create Deployment**.
3. The **Connect to Cluster0** page appears.
    - You do not need to create a database user at this time. You will do that later.
    - Click **Close**.
5. Now you will create your database.
    - Under **Database** in the upper-left click **Clusters**.
    - Click **Browse Collections**.
    - Click **+ Create Database**.
    - Set the **Database name** to `Todo` and the **Collection name** to `Tasks`.
    - Leave **Additional Preferences** unchanged.
    - Click **Create**.
6. Create a user that can access the database.
    - Under **Security** on the left side click **Database Access**.
    - Click the **+ Add New Database User** button.
    - Leave **Authentication Method** set to `Password`.
    - Enter a username and a password.
      - Username can be something like `todo-user`
      - Set a secure password or click **Autogenerate Secure Password**
    - **Record the password for later** (but do not put it somewhere that will be committed to your GitHub repo).
    - Under **Database User Privileges** click **Add Built In Role** and select `Read and write to any database`.
    - Click `Add User`
7. Configure network access.
    - Under **Security** on the left side click **Network Access**
    - It should show one network address that is authorized to access the cluster. It is the address you used when creating the cluster. To make development easier, we will allow access from any IP address. This means security will rely entirely on having the proper passwords and keys. A commercial deployment might lock things down more tightly.
    - Click **Add IP Address** in the upper-right.
    - Click **Allow Access From Anywhere**.
    - Click **Confirm**.
8. Get your connection string
    - Click **Overview** in the upper-left.
    - In the middle of the screen click **Connect**
    - Click **Drivers** in the middle of the screen
    - You can ignore everything here except the part where it says "Use this connection string in your application".
    - Copy the connection string. It starts with "mongodb+srv://"

In the connection string, replace `<db-username>` with the one you created in step 6, (e.g. `todo-user`), replace `<db-password>` with the corresponding password and right before the question mark (and after the slash) insert the name of your database.

When you are finished, your connection string should look something like this:<br/>
`mongodb+srv://todo-user:PW039FG3ABC@cluster0.orycp.mongodb.net/Todo?retryWrites=true&w=majority&appName=Cluster0`

Details will differ due to choice of location, the name of your cluster, etc.
Save that value. Later, you will put it in your **.env** file (which does not get saved in the repo due to `.gitignore`).

