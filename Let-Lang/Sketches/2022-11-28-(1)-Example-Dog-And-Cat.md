# Let-Lang Example

## Overriding, Overloading, Shadowing

~~~
//.let

let Petable = trait {
    fun Pet(): Unit
}

let Speakable = trait {
    fun Speak(let Out: ref Text-Writer): Unit
}

let Cat = class {

    // constructor
    // (no let, it binds the existing fields)
    new(name, happiness default 1)

    private let     name:      ref'this String
    private mutable happiness: Int32

    is Petable
    is Speakable

    override fun Pet(): Unit {
        // weird syntax for `happiness += 1`
        set happiness = it + 1
    }

    override fun Speak(let Out: ref Text-Writer): Unit {
        let message = happiness match {
            * until 0 -> "*hiss*"
            0 until 3 -> "*mew*"
            3 until * -> "*purr*"
        }
        Out.Write-Line(f"${name}: ${Message}")
    }

}

fun Cat-Main(let Env: ref Environment): Unit {
    let my-cat /*:own Cat*/ = new Cat("kitty")

    my-cat.Speak(Env.Console.Out)
    //> kitty: *mew*

    my-cat.Pet()
    my-cat.Speak(Env.Console.Out)
    //> kitty: *mew*

    repeat(10) {
        my-cat.Pet()
    }
    my-cat.Speak(Env.Console.Out)
    //> kitty: *purr*

    let petable-cat: ref Petable = my-cat
    pettable-cat.Pet()
//  pettable-cat.Speak(Env.Console.Out)
//> COMPILER ERROR: cannot find 'Speak' on 'Petable'!
}

let Dog = class {

    new(name, happiness default 1, wet-fur default False)

    private let     name:      ref'this String
    private mutable happiness: Int32
    private mutable wet-fur:   Boolean

    is Petable

    override fun Pet(): Unit {
        if (wet-fur) then {
            throw new Exception
        }
        set happiness = it + 1
    }

    is explicit Speakable {
        // explicit implementation
        Speak(let Out) {
            let message = happiness {
                * until 0 -> "*growl*"
                0 until * -> "*woof*"
            }
            let status = if (wet-fur) then " (wet)" else ""
            Out.Write-Line(f"${name}${status}: ${message}")
        }
    }

    fun Go-For-A-Walk(): Unit {
        // TODO: encapsulation and purity!?
        let rng = new Random
        if (wet-fur) then {
            if (rng.Next(0 until 100) < 10) then {
                set wet-fur = True
            }
        }
        else {
           if (rng.Next(0 until 100) < 20) then {
               set wet-fur = False
           }
        }
        set happiness = it + 5
    }

}

fun Dog-Main(let Env: ref Environment) {
    let my-dog: own Dog = new Dog("doggo")

    my-dog.Pet()

//  my-dog.Speak(Env.Console.Out)
//> COMPILER ERROR: cannot find `Speak` on `Dog`

    let speakable-dog: ref Speakable = my-dog
    speakable-dog.Speak(Env.Console.Out)
//> doggo: *woof*
}
~~~
