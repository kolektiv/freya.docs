Tutorials
=========

Articles
--------

.. note::

   We're working on some in-depth long-form tutorials, please keep an eye on this space.
   
Videos
------

We'll be producing short screencasts to complement the reference guides. To begin, here's a quick, broad introduction to Freya and developing an API.

* `Introduction to Freya (YouTube) <https://www.youtube.com/watch?v=TYvUovTP7qk>`_

Freya Hello World (from Introduction to Freya video)
----------------------------------------------------

.. code-block:: powershell

   Install-Package Freya
   Install-Package Microsoft.Owin.SelfHost

.. code-block:: fsharp

    module MyWebSite
    
    open System
    open System.IO
    open Freya.Core
    open Microsoft.Owin.Hosting

    let helloWorld =
        freya {
            let text = "Hello World"B
            let! state = Freya.State.get

            state.Environment.["owin.ResponseStatusCode"] <- 200
            state.Environment.["owin.RepsonseReasonPhrase"] <- "Awesome"
            state.Environment.["owin.ResponseBody"] :?> Stream
                |> fun x -> x.Write (text, 0, text.Length)
            }

    type Exercise() =
        member __.Configuration() =
            OwinAppFunc.ofFreya helloWorld

    [<EntryPoint>]
    let main _ =
        let url = "http://localhost:8080"
        WebApp.Start<Exercise> (url) |> ignore
        Console.WriteLine("Serving site at " + url)
        Console.WriteLine("Press ENTER to cancel")
        Console.ReadLine |> ignore

Simple Static File Server
-------------------------

.. code-block:: fsharp

    module SelfHostWebSite
    
    open Freya.Core
    open Microsoft.Owin.Hosting
    open System.IO
    open System

    //System.IO.Path.PathCombine is a bit weird
    let pathCombine a b =
      let rt = 
        if String.IsNullOrWhiteSpace a then 0
        elif a.EndsWith(string Path.DirectorySeparatorChar) then 1
        elif a.EndsWith(string Path.VolumeSeparatorChar) then 2
        else 3
      let fn = 
        if String.IsNullOrWhiteSpace b then 0
        elif b.StartsWith(string Path.DirectorySeparatorChar) then 1
        else 3
      match rt, fn with
      | 0, 0 -> ""
      | 0, _ -> b
      | _, 0 -> a
      | 1, 1 -> a + b.Substring(1)
      | 3, 3 -> a + (string Path.DirectorySeparatorChar) + b
      | _    -> a + b

    let fileServer root = freya {
      let! state = Freya.State.get
      let reqPath = state.Environment.["owin.RequestPath"]:?> String
      let path = 
        match state.Environment.["owin.RequestPath"] with
        | :? string as p when p <> "" && p <> "/" -> pathCombine root p
        | _                                       -> pathCombine root "index.html"
      let (code, phrase, bytes) =
        try
          match File.Exists path with
          | true  -> (200, "OK"                     , File.ReadAllBytes(path))
          | _     -> (404, "File Not Found - "      , [||])
        with _    -> (501, "Internal Server Error"  , [||] )
      state.Environment.["owin.ResponseStatusCode"] <- code
      state.Environment.["owin.ResponseReasonPhrase"] <- phrase
      state.Environment.["owin.ResponseBody"] :?> Stream
        |> fun s -> s.Write(bytes, 0, bytes.Length)
      }

    type Exercise() =
        member __.Configuration() =
            OwinAppFunc.ofFreya (fileServer @"C:\documents\myWebsiteStaticContentRoot\")

    [<EntryPoint>]
    let main _ =
      try
        let url = "http://localhost:8080"
        let _ = WebApp.Start<Exercise> (url)
        [ 
          "Serving site at " + url
          "Press ENTER to cancel"
        ] |> List.iter Console.WriteLine
        let _ = Console.ReadLine()
        0
      with x ->
        Console.WriteLine()
        Console.WriteLine x.Message
        Console.WriteLine()
        let _ = Console.ReadLine()
        1
