# Using SSH key

If the two-factor auth is activated in GitHub, then the HTTPS may not be used as origin URL. It is better to use SSH as origin URL so that SSH key can ensure the connection.

## Generate SSH key and add to account

```
$ssh-keygen -C example@example.com
```
Open the key file:
```
$cat ~/.ssh/id_rsa.pub
```
Copy the content of the generated pub file to GitHub account **Settings->SSH and GPG keys**.

Then test the connection:
```
$ssh -T git@github.com
```

If your username is shown, it means that the connection is established.

## Change origin from HTTPS to SSH

Check if the remote is using HTTPS:
```
$git remote -v
```

If not, then change it:
```
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Then it is done!