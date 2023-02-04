* YT Videos (UnityCon)
  https://www.youtube.com/playlist?list=PLmmYSbUCWJ4x1GO839azG_BBw8rkh-zOj
* 1
- Online: https://www.youtube.com/watch?v=Wibk0IfjfaI
- Book: Implementation Patterns, by Kent Beck
  https://www.goodreads.com/book/show/781559.Implementation_Patterns
- Book: Object oriented analysis & Design with applications, by Grady Booch
  https://www.goodreads.com/book/show/424923.Object_Oriented_Analysis_and_Design_with_Applications
- Book: Working Affectively with Legacy Code, by Michael Feathers
  https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
** "The productivity trap"
- Of how developing on a new project **green field** is fast at the
  the beggining but then it gets slower and slower.
- Due the "mess" build into the code.
- The harder you work the slower you go (in this scenarios)
- **Brooke's Law** applies here, as if you add programers to a late project makes it later
  Problem is also that old people and old code teach the new people.
** "The Big redesign in the sky"
- Start again, redesign
- Starts promising, tho usually in charge of a "tiger team" (people who made the original thing)
  Problem is that requirements are clutter into the "mess" of the current code, no documented.
  These creates two separate systems for a while that could last years until both are on par.
- It doesn't work

** "Code rot"
- "It was more complicated than I thought"
- 1) Rigidity: resist change. Requires many changes to fix a bug or add a new behaviour.
     Fragility: break when small changes are made.
- 2) Inseparability: parts of the system can't be re-used in other parts of other systems.
     Are impredictable, you never know if you can re-use components.
- 3) Opacity: offuscated coded, bad designed
** "Why does code rot?"
- We write it
- If we rush we would slow down
- The only way to go fast is to keep **clean code**
** "What is clean code?"
- C++ Creator
  - elegant (does a lot in few word)
  - efficient (uses few cpu cycles)
  - should do one thing
- Grady Booch
  - simple and direct
  - reads like well written prose
- Michael Feathers
  - looks like is written for someone that cares
- Cunningam, Inventor of Wiki's
  - when every routine you reads what you expected
** Addenum
- "The boy scout rule": leave the code better than we found it
- "Period luminosity relationship"
  - Given the period you can predict the luminosity.
  - Discovered from pulsating stars in Magallanes clouds.
  - You can measure the distance with luminosity and period.

* 2 - Names
- Article: Ottinger's Rules for Variable and Class Naming
  By Tim Ottinger
  https://www.maultech.com/chrislott/resources/cstyle/ottinger-naming.html
** Story: "Distance to the sun"
- Distance from earth to sun, Aristarco de Samos.
  Using a triangle between earth, moon and sun.
  Distance to the moon was calculated using:
  - the projection of the earth on the moon during eclipses
  - Earth's circumference
** "Reveal your intent"
- If your name needs a comment, then the name doesn't sufficient reveal intent.
- Adding **explanatory variables** can add intent.
  Form a kind of "compilable comment".
- True cost of software nowadays is on maintenance.
** "Describe the problem"
- Revealing implementation does NOT reveal intent.
- When you have to read the code in order to understand the name, the name failed.
- Communicating intent is the 1st priority, even before that if the code works.
** "Avoid Desinformation"
- Do not give names that create confusion.
- The name should be as abstract as the class.
- If the meanings of a function/class/variable changes, you should change the name
** "Pronounceable Names"
- Convenient for the authors AND readers
- Not getYYYY, PC_GWDA, m_qdox(), ppp()
  Not qty_tests, qty_pass, qty_pass_s, qty_skip, qty_fail
** "Avoid encodings"
- Like made up prefixes for types (especially on languages with type checking at compilation)
    they obscure the code
** "Parts of speech"
| type              | name       | example                |
|-------------------+------------+------------------------|
| Enums             | adjetives  | isEmpty                |
| Booleans          | predicates |                        |
| Methods           | verbs      | postPayment/isPostable |
| Variables/Classes | nouns      |                        |
- Ignore/avoid some words like: "manager", "processor", "data" or "info"
- do not use **nouns** for accessor (? use the **verb** getFirstName
- properties in c# are methods pretending to be vars so they are **Nouns**
- well writen prose
* 3 - Functions
- "Find all the classes on a design."
  - Classes might be hidding in large functions
- "A function should do only 1 thing, do it well, and do it only"
** Story: What makes the sun shine
- Kelvin: "The sun could derive his power from his own gravitational colapse.
           It only needs to contract 15ms century to keep his energy output."
- 1895 Bill Hermer? got the first Xray from his hand
- Radioactivity is the source of the sun's power
** The first rule of functions
- Small
  - up to 8 lines
  - originally a screenfold: originally about 20 lines
  - The argument for large functions lies on the recognition of the "shape" of the function
     - It works for regulars, but new people will get lost
    - Also from "efficiency" arguments
      - Holds true, if only, on the core loop of your program
  - A long function is where classes go to hide
* TODO 4 - Function Structure
People:
- "Structured Programming" - Edgard Dijkstra, Hoare, Dull
- Cunningham's Rule?
  - Kepp the code what you expect
- ="Avoid double takes"=
** function signature should be small
  - each argument might confuse
  - each argument can break the flow
  - 3 max arguments
  - same for constructors
    - use setters
    - or builder pattern
    - or a map/struct
** what _types_ should be passed
  - no booleans: the function does 2 things
  - avoid output arguments (in java, clojure, ruby there is no need)
  - null: also 2 behaviours/2 functions
** ~defensive programming~ is a code smell
  - in public apis is fine
  - test should cover the rest
** ~scissor rule~ (is NOT the convention in Java)
  - public at the top
  - private at the bottom
** ~stepdown rule~
  - identation each stepdown of methods
    or even having a language with true inner functions like ALGOL (or js)
  - no backwards references
  - public (static?) variables
    private variables
    public methods
    private methods...
  - not necesarilly ALL public functions at the top, but is divided into "steps" where each step has a public method
** self evident function structures
** switch/if
- switch statements are a missed opportunity to use _polymorphism_
  switch statements are _not OO_
- one of the main benefits of OO is the ability to manage _dependencies_
 | Flow control (aka runtime dependency) | eg a code from module A that calls a function from module B |
 | Source code dependency                | eg the import/include in module A that refers to module B   |
- OO allows us to invert the _source code dependency_ by using an interface
  - which makes it able to compile the modules separately, aka _independent deployability_
- Switch's =fan out problem=
  - each case of it, is likely to have a dependency outwards on an external module
  - forces recompile/redeploy if the modules in the switch change
- Solution:
  - replace the switch with a _baseclass_ with a method that corresponds with the action of the switch
  - each case becomea a _derived class_
** Main
- in every application you write, you should be able to draw a line through the module diagram
  - that separates
    1) App: the core application functionality
    2) Main: from the low level details, should be kept small and with not much subdivision
  - =Dependency Injection=
    the main partition should depend on the app partition, NOT the other way around
** TODO FP 00:42:00
- first to be invented
- no side effects
  - when there are side-effects
    - a function might change the behaviour of a function or some other functions
    - =temporal coupling=
      functions with it come in pair set/get, open/close, new/delete, called in order
    - 
- immutable
- no assigment
- recurse instead of looping
** SP
** clean error handling
** Story: Sun Nucleo
- the internal of the sun is so hot that the ripped the electrons of the hidrogen, exposing the protons (+)
- the force of magnet repulsion is inversing proportional to the square of the distance
- other force "strong nuclear force" is stronger and attracts them, but is distance limited
- the attraction could lead to _fusion_, which releases energy
- 4M tons of hidrogen _fuse_ into helium every second
* 6 - TDD Test Driven Development (Part 1)
- Code Rot
  - When you touch some code it becomes "yours".
* 8 - Foundations of SOLID
