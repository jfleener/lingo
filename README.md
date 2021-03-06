lingo
=====

Very basic Golang library for i18n. There are others that do the job, but this is my take on the problem.

Features:
---------
1. Storing messages in JSON files.
2. Support for nested declarations.
2. Detecting language based on Request headers.
3. Very simple to use.

Usage:
------
  1. Import Lingo into your project
  
      ```go
        import "github.com/kortem/lingo"
      ```
  1. Create a dir to store translations, and write them in JSON files named [locale].json. For example:
  
      ```    
        en_US.json
        sr_RS.json
        de.json
        ...
      ```
      You can write nested JSON too.
      ```json
        {
          "main.title" : "CutleryPlus",
          "main.subtitle" : "Knives that put cut in cutlery.",
          "menu" : {
            "home" : "Home",
            "products": {
              "self": "Products",
              "forks" : "Forks",
              "knives" : "Knives",
              "spoons" : "Spoons"
            },
          }
        }
      ```
  2. Initialize a Lingo like this:
  
      ```go
        l := lingo.New("default_locale", "path/to/translations/dir")
      ```
      
  3. Get bundle for specific locale via either `string`: 
  
      ```go
        t1 := l.TranslationsForLocale("en_US")
        t2 := l.TranslationsForLocale("de_DE")
      ```
      This way Lingo will return the bundle for specific locale, or default if given is not found.
      Alternatively (or primarily), you can get it with `*http.Request`:
      
      ```go
        t := l.TranslationsForRequest(req)
      ```
      This way Lingo finds best suited locale via `Accept-Language` header, or if there is no match, returns default.
      `Accept-Language` header is set by the browser, so basically it will serve the language the user has set to his browser.
  4. Once you get T instance just fire away!
  
      ```go
        r1 := t1.Value("main.subtitle")
        // "Knives that put cut in cutlery."
        r1 := t2.Value("main.subtitle")
        // "Messer, die legte in Besteck geschnitten."
      	r3 := t1.Value("menu.products.self")
        // "Products"
        r5 := t1.Value("error.404", req.URL.Path)
        // "Page idnex.html not found!"
      ```

TODO:
-----
  1. Feature to allow you to get T of specific segment in JSON, and not just string values. That way you can
      group specific messages and extract only that segment where you need it, of different views and/or components.
  2. Create a tool to keep all .json files in sync.
  2. Provide `http.HandlerFunc` that serves web pages for easy translations management.

Notice:
-------
I am a gopher newb, been hacking around for a couple of months in my spare time, so any comment/contrubution is welcome.
That said, use at your own risk. :D
