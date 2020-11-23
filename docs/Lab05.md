# Lab05: working with chart and repository 

1. ensure that you have configured the values.yaml in the wordpress chart, then put a new description and package your chart 

```
:~$ vi ./wordpress/Chart.yaml
```

put "Custom wordpress application for training" in the 'description' field, and "8.0.3" for the 'version' field.             
```                              
:~$ helm package ./wordpress
Successfully packaged chart and saved it to: /home/student/wordpress-8.0.3.tgz
```
now you need to move the packed chart in a repo directory and generate the index.yaml file
```
:~$ mv wordpress-8.0.3.tgz testing-repo
:~$ helm repo index testing-repo/
```
now you need to upload your directory in a HTTP repository,
you can use GCS, GitHub or custom HTTP server

Crete a Github Repository with a githubpages link.

```
:~$ gitclone <URL link>
```
now you need to move the packed chart in a repo directory and generate the index.yaml file
```
:~$ mv wordpress-8.0.3.tgz <github_repo-folder>
:~$ helm repo index <github_repo-folder>
```
Use the github commands to upload your changes:
```
:~$ cd <github_repo-folder>
:~$ git add .
:~$ git commit
```
login if prompted and put a commit message to push
```
:~$ git push
```


when you have uploaded your repo directory, you can add the repo on your helm client with a custom name and install directly from your repo:
```
:~$ helm repo add mypersonalrepo https://<your_repo_link>
```
verify the charts in your repo:

```
:~$ helm repo list
NAME          	URL                                   
bitnami       	https://charts.bitnami.com            
mypersonalrepo	https://<your_repo_link>
:~$ helm search mypersonalrepo
```

install directly from your repo:

```
:~$ helm install my-repo-wp mypersonalrepo/wordpress
```

2. Create a Chart 

```
:~$ helm create my-chart
Creating my-chart
```
check the chart directory
```
:~$ ls ./my-chart
charts  Chart.yaml  templates  values.yaml
```
you can check the tipical basic chart's files containing a nginx application,
you have a base for create a custom charts by modifying the manifest templates and values to pass it, and be confind with the GO template language when you have finished you can store it in a repo.


3. Challenge:

Create a Chart and describe your application in Chart.yaml file, then add it to your repository.










