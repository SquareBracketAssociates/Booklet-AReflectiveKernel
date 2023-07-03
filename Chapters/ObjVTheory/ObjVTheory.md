## A minimal reflective class-based kernel
	 return apply (lookup (selector classof(receiver) receiver) receiver arguments)
   if the method is found in class
      then return it
      else if class == Object
           then send the message error to the receiver
           else lookup (selector superclass(class) receiver)
   if the method is found in class
      then return it
      else if class == Object
           then return nil
           else lookup (selector superclass(class))
   methodOrNil = lookup (selector classof(receiver)).
   if methodOrNil is nil
      then return send the message error to the receiver
      else return apply(methodOrNil receiver arguments)
	union (instance-variables(superclass(aClass)), local-instance-variables(aClass))
>>> 10
B new foo
>>> 50
A new bar
>>> 10
B new bar
>>> 50
>>> 20
R new bar
>>> 100
>>> aPoint (24 6)
Point new :y 6 :x 24
>>> aPoint (24 6)
>>> aPoint (nil nil)
>>> aPoint (nil 10)
   :name 'Point'
   :super 'Object'
   :ivs #(x y)
>>> aClass
>>> #(Point nil nil)
>>>#(Class nil nil nil nil nil)
>>> #(Point nil nil) initialize: (:y 6 :x 24)]
>>> #(Point 24 6)
	:name 'AbstractMetaclass'
	:super 'Class'
	addMethod: #new
	body: [ :receiver :initargs | receiver error: 'Cannot create instance of class' ]
>>> Cannot create instance of class
>>> #(class name super ivs keywords methodDict unique)
	:name 'MetaPoint'
	:super 'Class'
	:ivs #(char)
	:name Point
	:super 'Object'
	:ivs #(x y)
	:char '*'
	:name Point
	:super 'Object'
	:ivs #(x y)
	:sharedivs {#char -> '*'}