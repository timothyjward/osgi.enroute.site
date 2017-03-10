---
---

Gogo is a surprising powerful shell in a very tiny package. It is used in virtually all OSGi installations that I meet. Newcomers to OSGi often love the shell to explore and navigate the environment. However, when I look at open source Gogo commnands they often look like:

    public String devices( String cmds[] ) {
        int wait = 10;
        boolean localOnly = false;
        
        int i = 0;
        while ( i < cmds.length && cmds[i++].startWith("-")) {
          switch( cmds[i] ) {
          case "--":
             break;
          case "-w":
          case "--wait":
              wait = Integer.parseIn(wait);
              break;
          case "-l":
          case "--localOnly":
              localOnly = true;
              break;
          case "-l":
          case "--limit":
              limit = Integer.parseIn(wait);
              break;
          }
        }
        try (Formatter f = new Formatter();) {
          while( i < cmds.length ) {
              String deviceId = cmds[i++];
              List<Device> ds= driver.findDevices( wait );
              for ( Device d : ds ) {
                f.format("Name           %s\n" +
                         "Address        %s\n" +
                         "Capacity       %s\n", 
                         deviceId, d.getAddress(), d.getCapacity());
              }
          }
          return f.toString();
        }
    }

Sadly, this is not the way you should write commands for Gogo. Though Gogo feels like a normal shell it actually is a scripting language with real objects. When you write Gogo commands you should accept Java objects and not just strings and for the return value it is the same. Out of the box Gogo supports many useful types and it provides an extension mechanism for custom types.

In the upcoming sections we will take  a tour of how to write Gogo commands and at the end come back and rewrite this example.

## Component

The skeleton of a Gogo command is as follows:

    @Component(
      service = GogoCommand.class,
      property = {
        Debug.COMMAND_SCOPE + "=scope", 
        Debug.COMMAND_FUNCTION + "=function"
    })
    public class GogoCommand
    {
       @Descriptor("Description of the command")
       public double command( 
                @Descriptor("Description of the argument") 
                double times
       )
       {
          return times;
       }
    }

The `COMMAND_SCOPE` defines the scope (surprise!). The `COMMAND_FUNCTION` is the name of the function as it will be used from the command line. If there is no method with that name Gogo will apply the bean patterns to for example a `getFoo()` method will be an implementation for a `foo` command.

And Yes, we are aware that setting properties on a component this way sucks. 

## Arguments

If there is one principle behind Gogo then it is the fact that you should normal plain old type safe java. Despite its tiny size, Gogo takes care of all type conversions and formatting behind the scenes. If you therefore need an `int` then just specify an `int`:

    @Descriptor("Demonstrate the use of type conversion")
    public double arg( 
            @Descriptor("Integer conversion") 
            int times
    )
    {
        return times;
    }

In general, this works for all applicable types in the VM and the OSGi specification. For example, if you want have a Bundle object then just specify a `Bundle` object:






If you write a command then you should define your arguments 


      public List<Device> devices( 
              @Parameter( names={"-l", "--localOnly} boolean localOnly
              
              ) {

The Gogo command shell is a lot more powerful than most people are aware of. In the field, most commands 
are written as Gogo is like bash: based on strings. However, Gogo is based on _objects_. The primary purpose
of Gogo was to create a shell that had the convenience of bash from the command line with the power of a scripting
language but without the hassle. The primary idea behind Gogo was that plain old type safe Java code was doing the work
while Gogo then coerced the command line into the right types for normal Java method calls and then
converted the returned objects to readable things. Unfortunately, looking at implementations of Gogo commands
very few people seem to be aware of this model and implement their 

Though the `<init>(String)` to construct an object from a string and `toString` to format it are great friends of Gogo, this would clearly not suffice. For this reason
