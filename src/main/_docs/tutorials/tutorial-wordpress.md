---
tags: [ "eclipse" , "che" ]
title: Wordpress
excerpt: "Create a new Wordpress project with a dedicated database"
layout: tutorials
permalink: /:categories/wordpress/
---
{% include base.html %}
# 1. Start Che  
Use your SaaS account for the following, or if you have [installed Che]({{base}}{{site.links["setup-getting-started"]}}), open a terminal and use the Che startup script:

```shell  
# Interactive help
docker run -it eclipse/che start

# Or, full start syntax where <path> is a local directory
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v <path>:/data eclipse/che start
```

When you execute this command, you'll see the URL for the user dashboard.

The Che dashboard will open. It is where you manage your projects and workspaces. If you do not have any projects in Che, you'll be asked to create a new project.  If you already have projects in Che, click on `New Project` button in the menu bar.

# 2. Import a Wordpress Project  
Click `New Project` and import a Wordpress repository and set up the right environment for it.

### Select Source
Let's import WP from an [official WordPress GitHub repo](https://github.com/WordPress/WordPress.git).

### Select Stack
Che offers a certified PHP image with Apache2, MySQL, git, curl and a number of popular PHP extensions.
[Dockerfile](https://github.com/eclipse/che-dockerfiles/blob/master/recipes/php/ubuntu/Dockerfile).

### Configure Workspace
Give your workspace a nice name and configure RAM, i.e. apply the memory limit to a workspace machine.

### Create the Project
Select `Create Project`.  The project construction process goes through a number of steps including creating a new workspace, downloading an image to use for the workspace environment, instantiating that environment, installing Che services into the workspace, and then creating your project.

Click the `Open in IDE` button to open your project in the IDE.

# 3. Start MySQL Server  
After the project is successfully created, you can start MySQL server either in the terminal (you can find it on the Consoles panel and open it by clicking `+` button) or using a command widget (CMD) - `sudo service mysql start`

We have already created a sample DB in the base image:
```text  
ENV CHE_MYSQL_PASSWORD=che
ENV CHE_MYSQL_DB=che_db
ENV CHE_MYSQL_USER=che
```

You may verify that the db is there and MYSQL is running: `mysql -u che -p che_db`

# 4. Run Your Project  

Let's start Apache server. You can do that in the terminal, however, using a Commands Widget is a better option. Go to `CMD > Edit Commands…` menu, click `+` button at Custom section and paste your command name, command itself here.
Here are some commands that you may use:  
```shell  
# Apache start
  sudo service apache2 start && sudo tail -f /var/log/apache2/access.log -f /var/log/apache2/error.log

# Apache restart
  sudo service apache2 restart

# Apache stop
  sudo service apache2 stop
```
Preview URL is another important object. It will be `http://${server.port.80}/${current.project.relpath}`.

`${server.port.80}` returns host and port, while `${current.project.relpath}` will be interpreted as a relative path to your project (relative to `/projects` which is DocumentRoot for Apache in this environment).

![Screenshot2.png]({{base}}{{site.links["Screenshot2.png"]}})

Run Apache, click the preview URL and find your WordPress project started there. Follow the WordPress guide to start your first site.

![Screenshot3.png]({{base}}{{site.links["Screenshot3.png"]}})

You will be prompted to connect your MySQL database (see details above). You can also connect an external database.

A few steps to install WP and voilà, it’s done.

# 5. Snapshot Workspace  

The last step is to save your workspace.

Go to Machine Perspective, choose `Machine > Create Snapshot`. It will create a new image which will be used to load the workspace, based upon the current contents of the environment.
