## Building a minimal reflective class-based kernel
  baseline: 'ObjV';
  repository: 'github://Ducasse/ObjVLispSkeleton/tree/master/src';
  load
>>> #(#ObjClass 'ObjPoint' #ObjObject #(class x y) #(:x :y) ... )
>>> #(#ObjClass 'ObjPoint' #ObjObject #(class x y) #(:x :y) ... )
   "(self selector: #testPrimitiveStructureObjClassId) run"

   self assert: (pointClass objClassId = #ObjClass).
   "(self selector: #testPrimitiveStructureObjIVs) run"

   self assert: ((pointClass objIVs) = #(#class #x #y)).
 #testPrimitiveStructureObjClassId) run`.  Note that arrays start at 1 in Pharo. Below is the list of the primitives that you should implement.
   "(self selector: #testClassAccess) run"

   self assert: (aPoint objClass = pointClass)
   "(self  selector: #testIVOffset) run"

   self assert: ((pointClass offsetFromClassOfInstanceVariable: #x) = 2).
   self assert: ((pointClass offsetFromClassOfInstanceVariable: #lulu) = 0)
   "(self  selector: #testIVOffsetAndValue) run"

   self assert: ((aPoint offsetFromObjectOfInstanceVariable: #x) = 2).
   self assert: ((aPoint valueOfInstanceVariable: #x) = 10)
   "(self  selector: #testAllocate) run"
   | newInstance |
   newInstance := pointClass allocateAnInstance.
   self assert: (newInstance at: 1) = #ObjPoint.
   self assert: (newInstance size) = 3.
   self assert: (newInstance at: 2) isNil.
   self assert: (newInstance at: 3) isNil.
   self assert: (newInstance objClass = pointClass)
   "(self  selector: #testKeywords) run"

   | dummyObject |
   dummyObject := Obj new.
   self assert:
      ((dummyObject generateKeywords: #(#titi #toto #lulu))
        = #(#titi: #toto: #lulu:)).
   self assert:
      ((dummyObject keywordValue: #x
          getFrom: #(#toto 33 #x 23)
          ifAbsent: 2) = 23).
   self assert:
      ((dummyObject keywordValue: #x
         getFrom: #(#toto 23)
         ifAbsent: 2) = 2).
   self assert:
      ((dummyObject returnValuesFrom: #(#x 22 #y 35) followingSchema: #(#y #yy #x #y))
         = #(35 nil 22 35))
   "(self  selector: #testInitialize) run"

   | newInstance  |
   newInstance := pointClass allocateAnInstance.
   newInstance initializeUsing: #(#y: 2 #z: 3 #t: 55 #x: 1).
   self assert: (newInstance at: 1) equals: #ObjPoint.
   self assert: (newInstance at: 2) equals: 1.
   self assert: (newInstance at: 3) equals: 2.
   "(self  selector: #testInstanceVariableInheritance) run"

   "a better choice would be to throw an exception if there are duplicates"
   self assert:
      ((Obj new computeNewIVFrom: #(#a #b #c #d) asOrderedCollection
         with: #(#a #z #b #t) asOrderedCollection)
         = #(#a #b #c #d #z #t) asOrderedCollection).
   self assert:
      ((Obj new computeNewIVFrom: #() asOrderedCollection
         with: #(#a #z #b #t) asOrderedCollection)
         = #(#a #z #b #t) asOrderedCollection)
   addUnaryMethod: #accessInstanceVariableX
   withBody: 'objself send: #getIV withArguments: #(#x)'.
ObjVLisp: objself send: #odd withArguments: #()

Pharo Binary: a + 4
ObjVLisp: a send: #+ withArguments: #(4)

Pharo Keyword: a max: 4
ObjVLisp: a send: #max: withArguments: #(4)
   self foo: x
   addMethod: #bar:
   args: 'x'
   withBody: 'objself send: #foo: withArguments: #x'.
   "(self  selector: #testMethodManagment) run"
   self assert: (pointClass doesUnderstand: #x).
   self assert: (pointClass doesUnderstand: #xx) not.

   pointClass
      addMethod: #xx
      args: ''
      withBody: 'objself valueOfInstanceVariable: #x '.
   self assert: (((pointClass bodyOfMethod: #xx) value: aPoint) = 10).
   self assert: (pointClass doesUnderstand: #xx).
   pointClass removeMethod: #xx.
   self assert: (pointClass doesUnderstand: #xx) not.
   self assert: (((pointClass bodyOfMethod: #x) value: aPoint) = 10)
   "Execute the method found starting from aClass and whose name is selector.
   The core of the sending a message, reused for both a normal send or a super one."
   | methodOrNil |
   methodOrNil := aClass lookup: selector.
   ^ methodOrNil
      ifNotNil: [ methodOrNil valueWithArguments: (Array with: self) , arguments ]
      ifNil: [ Error signal: 'Obj message' , selector asString, ' not understood' ]
   "send the message whose selector is <selector> to the receiver. The arguments of the messages are an array <arguments>. The method is looked up in the class of the receiver. self is an objObject or a objClass."

   ^ self basicSend: selector withArguments: arguments from:  self objClass
   "(self  selector: #testNilWhenErrorInLookup) run"

   self assert: (pointClass lookup: #zork) isNil.
   "The method zork is NOT implement on pointClass"
   "(self  selector: #testRaisesErrorSendWhenErrorInLookup) run"

   self should: [ pointClass send: #zork withArguments: { aPoint } ] raise: Error.
   "Open a Transcript to see the message trace"
   super foo: x
  addMethod: #bar:
  args: 'x'
  withBody: 'objself super: #foo: withArguments: #(#x) from: superClassOfClassDefiningTheMethod'.
ObjVLisp:  objself super: #odd withArguments: #() from: superClassOfClassDefiningTheMethod

Pharo Binary: super + 4
ObjVLisp: objself super: #+ withArguments: #(4) from: superClassOfClassDefiningTheMethod

Pharo Keyword: super max: 4
ObjVlisp: objself super: #max: withArguments: #(4) from: superClassOfClassDefiningTheMethod
     [ :objself :otherArgs  |
           ... method code ...
           ]]
method value: (Obj giveClassNamed: self objSuperclassId)
   | methodOrNil |
   methodOrNil := (aClass lookup: selector).
   ^ methodOrNil
      ifNotNil: [ methodOrNil valueWithArguments: (Array with: self) , arguments ]
      ifNil: [ Error signal: 'Obj message' , selector asString, ' not understood' ]
   | methodOrNil |
   methodOrNil := (aClass lookup: selector).
   ^ methodOrNil
      ifNotNil: [ methodOrNil valueWithArguments: (Array with: self) , arguments ]
      ifNil: [ self sendError: selector withArgs: arguments ]
   addMethod: #error
   args: 'arrayOfArguments'
   withBody: 'Transcript show: ''error '', arrayOfArguments first.  ''error '', arrayOfArguments first'.
   "send error wrapping arguments into an array with the selector as first argument. Instead of an array we should create a message object."

   ^ self send: #error withArguments:  {(arguments copyWithFirst: selector)}
      "self bootstrap"

      self initialize.
      self manuallyCreateObjClass.
      self createObjObject.
      self createObjClass.
  "self manuallyCreateObjClass"

  | class |
  class := self manualObjClassStructure.
  Obj declareClass: class.
  self defineManualInitializeMethodIn: class.
  self defineAllocateMethodIn: class.
  self defineNewMethodIn: class.
  ^ class

   class
    addMethod: #initialize
    args: 'initArray'
    withBody:
      '| objsuperclass |
      objself initializeUsing: initArray.  "Initialize a class as an object. In the bootstrapped system will be done via super"
      objsuperclass := Obj giveClassNamed: objself objSuperclassId ifAbsent: [nil].
      objsuperclass isNil
        ifFalse:
          [ objself
            objIVs: (objself computeNewIVFrom: objsuperclass objIVs with: objself objIVs)]
        ifTrue:
          [ objself objIVs: (objself computeNewIVFrom: #(#class) with: objself objIVs)].
      objself
        objKeywords: (objself generateKeywords: (objself objIVs copyWithout: #class)).
      objself objMethodDict: (IdentityDictionary new: 3).
      Obj declareClass: objself.
      objself'

   class
      addUnaryMethod: #allocate
      withBody: 'objself allocateAnInstance'

   ^ (self giveClassNamed: #ObjClass)
      send: #new
      withArguments: #(#(#name: #ObjObject #iv: #(#class)))
   | objObject |
   objObject := self objObjectStructure.
   objObject addUnaryMethod: #class withBody: 'objself objClass'.
   objObject addUnaryMethod: #isClass withBody: 'false'.
   objObject addUnaryMethod: #isMetaclass withBody: 'false'.
   ...
   ...
   ^ objObject
   "self bootstrap"

   | objClass |
   objClass := self objClassStructure.
   self defineAllocateMethodIn: objClass.
   self defineNewMethodIn: objClass.
   self defineInitializeMethodIn: objClass.
   objClass
     addUnaryMethod: #isMetaclass
     withBody: 'objself objIVs includes: #superclass'.
  "an object is a class if is class is a metaclass. cool"

   objClass
     addUnaryMethod: #isClass
     withBody: 'objself objClass send: #isMetaclass withArguments:#()'.

   ^ objClass

   | objClass |
   objClass := self objClassStructure.
   self defineAllocateMethodIn: objClass.
   self defineNewMethodIn: objClass.
   self defineInitializeMethodIn: objClass.
   ...
   ^ objClass
   addUnaryMethod: #isMetaclass
   withBody: 'objself objIVs includes: #superclass'.

   "an object is a class if is class is a metaclass. cool"
  addUnaryMethod: #isClass
  withBody: 'objself objClass send: #isMetaclass withArguments:#()'.

   objClass
     addMethod: #initialize
     args: 'initArray'
     withBody:
        'objself super: #initialize withArguments: {initArray} from: superClassOfClassDefiningTheMethod.
         objself objIVs: (objself
                  computeNewIVFrom:
                        (Obj giveClassNamed: objself objSuperclassId) objIVs
                  with: objself objIVs).
        objself computeAndSetKeywords.
        objself objMethodDict: IdentityDictionary new.
        Obj declareClass: objself.
        objself'

  objClass
     addMethod: #initialize
     args: 'initArray'
     withBody:
         'objself super: #initialize withArguments: {initArray} from: superClassOfClassDefiningTheMethod.
         objself objIVs: (objself
           computeNewIVFrom: (Obj giveClassNamed: objself objSuperclassId) objIVs
           with: objself objIVs).
         objself computeAndSetKeywords.
         objself objMethodDict: IdentityDictionary new.
         Obj declareClass: objself.
         objself'
   withArguments: #((#name: #ObjPoint #iv: #(#x y) #superclass: #ObjObject)).
aPoint send: #getIV withArguments: #(#x).
aPoint send: #setIV withArguments: #(#x 25).
aPoint send: #getIV withArguments: #(#x).
   addUnaryMethod: #givex
   withBody: 'objself valueOfInstanceVariable: #x '.
Obj ObjPoint
   addUnaryMethod: #display
   withBody:
    'Transcript cr;
      show: ''aPoint with x = ''.
    Transcript show: (objself send: #givex withArguments: #()) printString;
   cr'.
aPoint send: #x: withArguments: #(33).
aPoint send: #display withArguments: #().
   send: #new
   withArguments: #((#x: 24 #y: 6 #color: #blue)).
aColoredPoint send: #setIV withArguments: #(#x 25).
aColoredPoint send: #getIV withArguments: #(#x).
aColoredPoint send: #getIV withArguments: #(#color).
   withBody:
     'objself super: #display withArguments: #() from: superClassOfClassDefiningTheMethod.
      Transcript cr;
         show: '' with Color = ''.
      Transcript show: (objself send: #giveColor withArguments: #()) printString; cr'.
aColoredPoint send: #color withArguments: #().
aColoredPoint send: #display withArguments: #()
  send: #new
  withArguments: #(#(#name: #ObjAbstractPoint
            #iv: #()
            #superclass: #ObjPoint)).

ObjAbstractPoint send: #new
   withArguments: #(#(#x: 24 #y: 6))        "should raise an error"
or
Obj Workstation sharedVariableValue: #domain

and get
 #((domain 'inria.fr'))