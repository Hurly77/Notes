### Bundler

#Format for new gem on terminal

- [ ] ```ruby
  bundle gem name #use camel_case
  cd name
  open text editor . #don't forget the dot after it opens all thre files
  #rename
  cd name
  rn -rf name
  repeat above
  resource https://bundler.io/
  ```

  resource https://bundler.io/ 

There is two different kind of gems 

1. CLI gem

   [On How TO ]: https://robdodson.me/how-to-write-a-command-line-ruby-gem/

2. Libraries

   #### executable

   Is the first file new file in your bin call it "file_name"

   

   

```
#!/usr/bin/env ruby
```

> The shebang ```#!/usr/bin/env ruby``` tells the  os that it should use Ruby to execute our code.      `ps.**Bash** is a **command** processor`

 running file, you could run the file like so `ruby bin/file_name` but, relisticly you'd want it to look something like this `(which is through bash looking at the interpreter). /bin/file_name` because that is the way a user would run it to install a gem but, first you'll have to give it permission. like so

```
cd bin/ #this looks inside the bin directory
ls -lah #list of all atributes in humam readable form
total 7.0K
drwxr-xr-x 2 camer Administrators    0 Mar 22 20:07 .
drwxr-xr-x 6 camer Administrators 4.0K Mar 22 20:05 ..
-rwxr-xr-x 1 camer Administrators  332 Mar 22 20:08 console
-rwxr-xr-x 1 camer Administrators   32 Mar 22 20:07 new-gem
-rwxr-xr-x 1 camer Administrators  131 Mar 22 20:05 setup
r=read permissions w=write promissions
x=executive permissions
give path to give permissions .bin/file_name
```

```
#Our CLI controller
class NewGem::CLI

    def call
        puts "new_gem"
    end
```

