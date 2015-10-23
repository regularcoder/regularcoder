---
layout: post
title:  "TIL#9: Destructuring assignment in F#"
date:   2015-10-23 15:00:00
categories: TIL, F#
---

In F# you can use [fst](https://msdn.microsoft.com/en-us/library/ee370480.aspx) or [snd](https://msdn.microsoft.com/en-us/library/ee340353.aspx) to extract the first or second element respectively from a tuple. However if your tuple has more than two elements you won't be able to use those operators.

Instead, you can use a destructuring assignment. Here is an example:

{% highlight FSharp linenos %}
(* Destructuring assignment *)
let DestructuringAssignment() =
    //Create a tuple
    let employee = ("Javed", 39, "Kolkata")

    //Destructure the tuple
    let (name, age, location) = employee

    printfn "Through destructuring: "
    printfn "Name: %s" name
    printfn "Age: %d" age
    printfn "Location: %s" location

    System.Console.ReadKey() |> ignore
    ()

[]
let main argv =
    DestructuringAssignment()
    0
{% endhighlight FSharp linenos %}

The breaking of the tuple (i.e. destructuring) it into sub-components is achieved by this line:

{% highlight FSharp linenos %}
//Destructure the tuple
let (name, age, location) = employee
{% endhighlight FSharp linenos %}