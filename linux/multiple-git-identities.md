# Multiple git identities

For managing multiple git repositories owned by multiple git hosting site accounts, one can config the ssh identity per repo.

```
cd ~/workspace/repo_one
git config --add --local core.sshCommand 'ssh -i ~/.ssh/id_account_one -F /dev/null' 
git config --add -user.email user@accountOne.com
git config --add --user.name userOne

cd ~/workspace/repo_two
git config --add --local core.sshCommand 'ssh -i ~/.ssh/id_account_two -F /dev/null' 
git config --add -user.email user@accountTwo.com
git config --add --user.name userTwo
```

{% hint style="info" %}
ssh-agent should have all the identities or be empty. Have at least one identity in the ssh-agent will always override other identities
{% endhint %}

```
// Add all the identies so that the ssd agent retries with all the identies
ssh-add ~/.ssh/id_account_one
ssh-add ~/.ssh/id_account_two

==========OR=================
// remove all identies so that the identity provided by git is used
ssh-add -D
```
