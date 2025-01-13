# MongoDB Guide (MongoSH, compass)

# installation

To install full mongodb you need two packages to install

1. [MongoDB Community Server](https://www.mongodb.com/try/download/compass)
2. [MongoDB Shell](https://www.mongodb.com/try/download/shell)

## Post installation setup

Before start using mongo we need to meke it discoverable by your system.

- This step include editing env variable. If you are in windows your mongo executable files should be in somethink like `C:\Program Files\MongoDB\Server\6.0\bin`. your version can be diffrent. copy that path of that bin folder. search `Edit environment veriables` and open it.

- You will see a button called `environment variables..` in bottom right corner , click on it. Under the user variavle you see `path`. double click on it. It will open a new dialog window. now click on `Edit` and paste the copied bin path there. now click on ok -> ok -> ok.

- open a new cmd and type `mongosh` and hit `ENTER` . You should see your mongo instance started. If it throwing check that all the previous steps has been followed correctly and the environment variable has been set up correctly.
- Now your installation is complete.

### Optinal settings.

If you install Mongodb as service then it will start on boot despite of you dont need and it will eat some of your ram space for no reason. If you don't want to start mongo you should set some settings.

- Open Powershell with admin access.
- paste

```
sc config MongoDB start=demand
```

To revert settings

```
sc config MongoDB start=auto
```

Now form reboot mongoDB will not avalaible automatically.

- To start the service paste the command in Powershell Admin

```
net start MongoDB
```

- To stop the service paste the command in Powershell Admin

```
net stop MongoDB
```

## Compass Connection.

- Open MonghoDB Compass application.
- Click on `New Connection` and put `mongodb://localhost:27017` in `URI` field and click `Connect`. you should See all databases listed in side panel. You can Create a database or a collections.
