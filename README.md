### Watchdog

Watchdog is a tiny header only library that allows to watch files or directories. It is supposed to be  compatible with any application that supports c++11 and uses boost and/or cinder. 

**Watchdog is currently a work-in-progress.**

By default watchdog is disabled in release mode and will only execute the provided callback once when wd::watch is called and do nothing for the other methods. Undef WATCHDOG_ONLY_IN_DEBUG if you want Watchdog to work in release mode.
 
**For the moment non-Cinder application will have their callbacks called in a separated thread!**

The API is very small an only expose those 4 functions to watch, unwatch or touch files or directories:

``` c++
wd::watch( const fs::path &path, const std::function<void(const fs::path&)> &callback );
wd::unwatch( const fs::path &path );
wd::unwatchAll();
wd::touch();
```

You can use a wildcard character to only watch for the desired files in a directory :

``` c++
wd::watch( "shaders/lighting.*", []( const fs::path &path ){
	// do something
} );
```

If you need to be able to iterate through a watched directory you can change the callback signature to this :

``` c++
wd::watch( "images/*.png", []( const vector<fs::path> &paths ){
	for( auto p : paths ){
		cout << p << " has changed" << endl;
	}
} );
```

In the context of cinder both absolute path and path relative to the asset folder are accepted.

There's is also a method to update the last write time of a file or directory which is usefull if you want to force the update of some files:

``` c++
wd::watch( "shaders/include/*", []( const fs::path &path ){
	// this will trigger any watched asset callback in "shaders"
	wd::touch( "shaders" );
} );
wd::watch( "shaders/lighting.*", []( const fs::path &path ){
	// triggered on changes any from "lighting.*" and "include/*"
} );
wd::watch( "shaders/wireframe.*", []( const fs::path &path ){
	// triggered on changes any from "wireframe.*" and "include/*"
} );
```

##### License

 Copyright (c) 2014, Simon Geilfus
 All rights reserved.
 
 Redistribution and use in source and binary forms, with or without modification, are permitted provided that
 the following conditions are met:
 
 * Redistributions of source code must retain the above copyright notice, this list of conditions and
 the following disclaimer.
 
 THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED
 WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
 PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
 ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
 TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
 HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
 NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
 POSSIBILITY OF SUCH DAMAGE.
