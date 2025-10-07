## Git  
``` bash 

git init 
git add . 
git commit -m 'msg'
git branch -M main # if you want a new branch 
git remote origin <ssh link or http link >
git push 
git status 

```
## DVC
```bash 
dvc init 
dvc add ./data ./models/   # what i want to track 
dvc remote add --default myremote gdrive://folder id 
dvc push 
dvc status 



```

