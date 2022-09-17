# Fat-Trait-Refs

## Let-Lang (Pseudocode)

~~~
let Animal = trait {
  fun Speak(let Env :Environment) :Unit
}

let Petable = trait {
  fun Pet(let Env :Environment) :Unit
}

let Pet = trait {
  extends Animal
  extends Petable
}

let Cat = class {
  extends Pet

  override fun Speak(let Env :Environment) :Unit {
    Env.Echo("Meow!")
  }

  override fun Pet(let Env :Environment) :Unit {
    Env.Echo("Purr!")
  }
}
~~~

## C (Pseudocode)

~~~c
//
// (interface) Animal
//

struct Animal__trait {
  void * _class;
// up-casting
  Any__trait * _as_Any;
// methods
  void (*Speak)(Animal__ref this, Environment__ref Env);
};

struct Animal__ref {
  Animal__trait * _trait;
  void *          _data;
};


//
// (interface) Petable
//

struct Petable__trait {
  void * _class;
// up-casting
  Any__trait * _as_Any;
// methods
  void (*Pet)(Petable__ref this, Environment__ref Env);
};

struct Petable__ref {
  Petable__trait * _trait;
  void *           _data;
};


//
// (interface) Pet
//

struct Pet__trait {
  void * _class;
// up-casting
  Any__trait *     _as_Any;
  Animal__trait *  _as_Animal;
  Petable__trait * _as_Petable;
// methods
  void (*Speak)(Pet__ref this, Evironment__ref Env);
  void (*Pet)(Pet__ref this, Environment__ref Env);
};

struct Pet__ref {
  Pet__trait * _trait;
  void *       _data;
};


//
// (class) Cat
//

char Cat__class[] = {0};

struct Any__trait Cat__as_Any = {
  ._class = &Cat__class;
};

struct Animal__trait Cat__as_Animal = {
  ._class  = &Cat__class;
  ._as_Any = &Cat__as_Any;
  .Speak   = &Cat__as_Animal__Speak;
};

struct Petable__trait Cat__as_Petable = {
  ._class  = &Cat__class;
  ._as_Any = &Cat__as_Any;
  .Pet     = &Cat__as_Petable__Pet;
};

struct Pet__trait Cat__as_Pet {
  ._class      = &Cat__class;
  ._as_Any     = &Cat__as_Any;
  ._as_Animal  = &Cat__as_Animal;
  ._as_Petable = &Cat__as_Petable;
  .Speak       = &Cat__as_Pet__Speak;
  .Pet         = &Cat__as_Pet__Pet;
};

void Cat__as_Animal__Speak(
  Animal__ref      this,
  Environment__ref Env
) {
  if (this._trait != &Cat__as_Animal) {
    __ThrowClassCastException();
  }
  Cat_Speak(this._data, Env);
}

void Cat__as_Petable__Pet(
  Petable__ref     this,
  Environment__ref Env
) {
  if (this._trait != &Cat__as_Petable) {
    __ThrowClassCastException();
  }
  Cat_Pet(this._data, Env);
}

void Cat__as_Pet__Speak(
  Pet__ref         this,
  Environment__ref Env
) {
  if (this._trait != &Cat__as_Pet) {
    __ThrowClassCastException();
  }
  Cat_Speak(this._data, Env);
}

void Cat__as_Pet__Pet(
  Pet__ref         this,
  Environment__ref Env
) {
  if (this._trait != &Cat__as_Pet) {
    __ThrowClassCastException();
  }
  Cat_Pet(this._data, Env);
}

void Cat_Speak(void * data, Environment__ref Env) {
  Env._trait->Echo(Env, "Meow!");
}

void Cat_Pet(void * data, Environment__ref Env) {
  Env._trait->Echo(Env, "Purr!");
}
~~~
