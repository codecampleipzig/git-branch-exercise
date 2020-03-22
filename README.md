# Git Branch Exercise

## 1. Creating branches and fast forward merge

- Create a new branch called new-colors and check it out. There are two possibilities to do so:
  ```
  git branch new-colors
  git checkout new-colors;
  ```
  Or in one command
  ```
  git checkout -b new-colors
  ```
  Run `git branch -a` to verify that the new branch has been created and is checked out.
- Change the body font color in style.css to `#4e443c` the background-color to #f0efe7 and for h1, h2, h3, h4, h5, h6 set the font color to `#f14e32`
  ```css
  body {
    /* ... */
    color: #4e443c;
    background-color: #f0efe7;
  }
  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    color: #f14e32;
  }
  ```
  Open live server to verify the results.
- Stage and commit the changes
  ```bash
  git add .
  git commit -m "Change colors to match git-scm.com"
  ```
- Now jump back to the master branch and see how the colors change back to black and white.
  ```
  git checkout master
  ```
  Jump back and forth between `master` and `new-colors` to get used to the command.
- We are contend with the new color scheme and will now merge the changes into the master branch. First checkout the master branch. Run `git branch` or `git status` to verify.
- Next run `git merge new-colors`. Because there were no commits in the master branch since new-colors was created the merge will happen automatically as a **fast-forward** merge.
- Now the merge is completed. You will see that the master branch also has the new color scheme. Run git log to see the commit history for the master branch and notice how the commit from the new-colors branch was added.
- Now we don't need the new-colors branch anymore. Run this command to delete the branch
  ```
  git branch -d new-colors
  ```
  Run git branch to verify.

## 2. Undoing mistakes

One of the most useful capabilities of git is that once a file has been comitted, we can always jump back to it. Let's try this.

- Delete the index.html file

  ```
  rm index.html
  ```

  The index.html file should now be gone and the live server will be unable to still serve the page.

- To get the repository back into the state at the last commit, you can simply run:

  ```
  git reset --hard
  ```

  and the index.html will reappear.

## 3. Three Way Merge

Now we want to emulate a scenario where changes are happening on two branches in parallel and they have to be merged back.

- Since we now no how to create branches it's time to change the heading of the page. Create a new branch called `new-heading` and check it out. Then change the heading of the page in `index.html` to say: "git branching is simple"

```bash
git checkout -b new-heading
```

```html
<h1>git branching is simple</h1>
```

Stage and commit your changes.

- In the meantime one of our colleagues notices that the heading is starting with a lowercase letter and wants to fix it. Run `git checkout master` to go back to the master branch. Then go to the heading and change it to say:

```html
<h1>Git branching is hard</h1>
```

Stage and commit your changes.

```bash
git commit -am "fix: Change heading to start in uppercase"
```

- Now it's time to merge branches. While still sitting on the master branch run

  ```
  git merge new-heading
  ```

  This time the commit can't happen automatically. Instead git will enter marks in index.html around the line with both the version from master and new-heading.

  ```
  <<<<<<< HEAD
    <h1>Git branching is hard</h1>
  =======
    <h1>git branching is simple</h1>
  >>>>>>> new-heading
  ```

  Edit these lines to incororate both the uppercase Git as well as the change from hard to simple

  ```html
  <h1>Git branching is simple</h1>
  ```

- Now you can mark the merge conflicts in this file as resolved by adding the file to the staging area.

```
git add index.html
```

- All merge conflicts are resolved, time to do the merge commit

```
git commit
```

This will open up the editor with a default message describing the two branches that are being merged.

- Run `git log --graph` and notice how the branching out and the merge are visualized.

- Finally delete the new-heading branch
  ```
  git branch -d new-heading
  ```
