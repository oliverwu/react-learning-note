1. Why you can’t break a forEach loop in JavaScript

It's because the loop is running that callback function over every item, so even if you write a return it's only returning on that instance of the function. It keeps going. In the case of the forEach() function, it doesn't do anything with the returned code. Be aware, that is not the case for some of the other Array Methods.
Additionally, because of this, break or continue are not valid statements.