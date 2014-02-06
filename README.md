# Y.ModelList.more #

The sync layer for `Y.Model` & `Y.ModelList` instances is a nice convenienced, but I found myself frustrated that there wasn't a simple way to load in more results. I often use a `Y.ModelList` instance to represent search params & results, and being able to load in more results on-demand is super useful.

Thus, `Y.ModelList.more()`. It's really just a version of `.load()` that doesn't blow away the existing models. 

## Usage ##

The extension is mixed into `Y.ModelList` constructors just like any other extension using `Y.Base.create`.

```javascript
var ModelList = Y.Base.create("tweets", Y.LazyModelList, [
    extensions.ModelListMore,
], {
    ...
});
```
    
The extension adds a `.more()` method to the ModelList's prototype that will then be passed to the sync layer. it will also fire a `more` event when the sync has completed and the new items have been parsed & added to the ModelList instance.

## Example ##

```javascript
var ModelList = Y.Base.create("tweets", Y.LazyModelList, [
    extensions.ModelListMore,
], {
    sync : function(action, options, done) {
        if(action === "more") {
            ...
        }
    }
});

var ml = new ModelList();

ml.on("more", function(e) {
    // e.parsed is the new items added
});

// Callback is optional
ml.more({ fooga : "wooga" }, function(err, items) {
    if(err) {
        throw new Error("OH NOOOOOOOO");
    }
});
```

## License ##

```
Copyright (c) 2014 Patrick Cavit

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
