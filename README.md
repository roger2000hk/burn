# Burn

Burn is a free and open source framework that allows you to create 8-bit flavored application using Ruby DSL.

```ruby
scene do
  label "Hello, World!"
end
```

Just like Recipe and Cookbook are DSL for Chef rubygem, this dead simple DSL is for Burn rubygem, and we call it Fuel. Burning this Fuel will produce [this](http://www.example.com).

![Hello-world pic][http://k.swd.cc/burn/resource/screenshot/hello-world.png]

Here is another example. With Fuel DSL, you can even compose background music in 1 minute.

```ruby
scene do
  label "Hello, World!"
  play "openning"
end

music "openning" do
  tempo :allegro
  channel "piano" do
    segno
    g :dotted
    g :eighth, :staccato
    a :dotted
    a :eighth, :staccato
    dal_segno
  end
end
```

Check [the output](http://www.example.com) from this. 

Would you like to design retro 8-bit graphics? Here you go.

```ruby
declare do
  star <<-EOH
                
       11       
       11       
      1111      
      1111      
1111111111111111
 11111111111111 
  111111111111  
   1111111111   
    11111111    
    11111111    
    11111111    
   1111  1111   
   11      11   
  1          1  
                
EOH
end

scene do
  main_loop <<-EOH
    star.x=20
    star.y-=3
    sprite "star"
EOH
end

```

![star animated gif][http://k.swd.cc/burn/resource/screenshot/star.gif]

Please visit [our project page](http://k.swd.cc/burn/) for more example.

## Table Of Contents

* [Getting Started](#getting-started)
    * [How It Works](#how-it-works)
    * [Installation](#installation)
    * [Quick Start](#quick-start)
* [Fuel DSL Methods](#fuel-dsl-methods)
    * [Scene](#scene)
    * [Sound](#sound)
    * [Music](#music)
    * [Declare](#declare)
* [Burning Fuel DSL In Action](#burning-fuel-dsl-in-action)
    * [Programming With .rrb(Restricted Ruby) Syntax](#programming-with-rrbrestricted-ruby-syntax)
* [Notes](#notes)
    * [License](#license)
    * [ToDos](#todos)

## Getting Started

### How It Works

Internally, burn contains cc65 executables inside its gemfile and calls them to compile. Main workflow is as follows.

- translate ruby DSL file into c source code
- compile them to make executable(main.out) by calling cc65
- provide emulator(JSNES) for rpaid application development

This means what burn can do is always less than what cc65 can do.

### Installation

    gem install burn

### Quick Start

    echo "scene do{ label 'hello world'}" > main.rb
    burn -p  # -p: open up emulator right after compilation

## Fuel DSL Methods

Currently following DSL Methods are available. If you are newbie here, kindly visit [project page](http://k.swd.cc/burn/) first to see what you can do with Burn and Fuel DSL.

* [Scene](#scene)
* [Sound](#sound)
* [Music](#music)
* [Declare](#declare)

### Scene

#### label(string, x=0, y=1)

The label method can be used in a scene to display static string.

<dl>
  <dt>string String</dt>
  <dd>Static string to display.</dd>
  <dt>x Number</dt>
  <dd>The horizontal coordinate to display, between 0 and 31.</dd>
  <dt>y Number</dt>
  <dd>The vertical coordinate to display, between 0 and 28.</dd>
</dl>

```ruby
scene do
  label "Hello, World!"
  label "Hello, World!", 4, 5
end
```

#### fade_in, fade_out

These methods can be used in a scene to fade in or out.

```ruby
scene do
  label "Hello"
  fade_in
  main_loop
  fade_out
  goto "next"
end
```

#### play(song_title)

The play method can be used in a scene to play music.

<dl>
  <dt>song_title String</dt>
  <dd>Song title to play.</dd>
</dl>

```ruby
scene do
  play "battle"
  stop
end

music "battle" do
  channel "string" do
    g   :dotted
    g   :eighth
    c   :half
  end
end
```

#### stop

The stop method can be used in a scene to stop music.

```ruby
scene do
  stop "battle"
end
```

#### color(palette, color, lightness=:lighter)

The color method can be used in a scene to pick a color and set it to specific palette.

<dl>
  <dt>palette Symbol</dt>
  <dd>Palette to set.
    <table>
      <tr>
        <th>Palette Symbol</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>:bg</td>
        <td>Background color</td>
      </tr>
      <tr>
        <td>:text</td>
        <td>Foreground color</td>
      </tr>
      <tr>
        <td>:palette_x1, :palette_x2, :palette_x3</td>
        <td>Colors for :palette_x</td>
      </tr>
      <tr>
        <td>:palette_y1, :palette_y2, :palette_y3</td>
        <td>Colors for :palette_y</td>
      </tr>
      <tr>
        <td>:palette_z1, :palette_z2, :palette_z3</td>
        <td>Colors for :palette_z</td>
      </tr>
      <tr>
        <td>:sprite</td>
        <td>Sprite color</td>
      </tr>
    </table>
  </dd>
  <dt>color Symbol</dt>
  <dd>Color to set. Available color pattern is shown below.<br />
    <img src="http://www.thealmightyguru.com/Games/Hacking/Wiki/images/e/e8/Palette_NTSC.png">
    <table>
      <tr>
        <th>Color Symbol</th>
      </tr>
      <tr><td>:white</td></tr>
      <tr><td>:lightblue</td></tr>
      <tr><td>:blue</td></tr>
      <tr><td>:purple</td></tr>
      <tr><td>:pink</td></tr>
      <tr><td>:red</td></tr>
      <tr><td>:deepred</td></tr>
      <tr><td>:orange</td></tr>
      <tr><td>:lightorange</td></tr>
      <tr><td>:darkgreen</td></tr>
      <tr><td>:green</td></tr>
      <tr><td>:lightgreen</td></tr>
      <tr><td>:bluegreen</td></tr>
      <tr><td>:gray</td></tr>
      <tr><td>:black</td></tr>
    </table>
  </dd>
  <dt>lightness Symbol</dt>
  <dd>Lightness of the color to set. `:darkest`, `:darker`, `:lighter` and `:lightest` can be set.</dd>
</dl>

```ruby
scene do
  label "Hello, World!"
  color :text, :pink, :darker
end
```

#### wait(interval)

The wait method can be used in a scene to pause for certain period of time.

<dl>
  <dt>interval Fixnum</dt>
  <dd>Period of time to pause.</dd>
</dl>


```ruby
scene do
  label "foobar"
  fade_out
  wait 100
  goto "somewhere"
end
```

#### goto(scene_name)

The goto method can be used in a scene to jump the scene specified.

<dl>
  <dt>scene_name String</dt>
  <dd>Destination to jump.</dd>
</dl>

```ruby
scene "first" do
  label "good morning"
  goto "second"
end

scene "second" do
  label "hi there"
end
```

#### inline(code)

The inline method can be used in a scene to inject c source code manually to compile with cc65. Should be used for debugging purpose.

<dl>
  <dt>code String</dt>
  <dd>C code to inject.</dd>
</dl>

```ruby
scene do
  inline "x=1+2;"
end
```

#### screen(map, vars)

The screen method can be used to inflate map data to a scene.

<dl>
  <dt>map String</dt>
  <dd>Map data consists of hash keys of pattern design.</dd>
  <dt>vars Hash</dt>
  <dd>A list of pattern designs.</dd>
</dl>

```ruby
scene do
  screen <<-EOH, { A:"tile", B:"wave", C:"mountain"}
                                
 AAAA  AA     AAA    AAA  AA  A 
 AA  A AA    AA AA  AA AA AA A  
 AA  A AA   AA   AA AA    AAA   
 AAAAA AA   AA   AA AA    AA    
 AA  A AA   AAAAAAA AA    AAA   
 AA  A AA   AA   AA AA AA AA A  
 AAAA  AAAA AA   AA  AAA  AA  A 
                                
 BB   BB    BB   BBB   BBBBB    
 BB   BB    BB  BB BB  BB  BB   
  BB BB BB BB  BB   BB BB  BB   
  BB BB BB BB  BBBBBBB BBBB     
   BB    BB    BB   BB BB  BB   
   BB    BB    BB   BB BB  BB   
EOH
end
```

#### sound(effect_name)

The sound method in a scene can be used to play sound effect.

<dl>
  <dt>effect_name String</dt>
  <dd>Name of sound effect to play.</dd>
</dl>

```ruby
scene do
  sound "dead"
end

sound "dead" do
  effect do
    velocity 15
    length 5
    pitch 200
  end
end
```

#### paint(dest, palette)

The paint method in a scene can be used to associate color palette with pattern designs inflated on the screen. Typically #paint is called along with #color and #screen method.

<dl>
  <dt>dest Range</dt>
  <dd>Return value of #range(x_from, y_from, x_to, y_to) is expected. x_from and x_to takes the number between 0 and 255 while y_from and y_to takes the value between 0 and 239.</dd>
  <dt>palette Symbol</dt>
  <dd>Palette symbol to apply. Kindly refer candidate symbols listed at <a href="#colorpalette-color-lightnesslighter">color</a> section.</dd>
</dl>

```ruby
scene "title" do
  color :palette_x1, :deepred, :darker
  paint range("44px", "5px", "120px", "3px"), :palette_x # range takes String parameter

  color :palette_y2, :pink, :darkest
  paint range(30, 2, 30, 12), :palette_y # range method can take Number as well. In this case x_from and x_to takes the value between 0 and 31, while y_from and y_to takes the value from 0 and 29.
end
```

#### main_loop(rrb_source=nil)

The main_loop method in a scene can be used to repeat a block of code. More detail can be found at [.rrb section](#programming-with-rrbrestricted-ruby-syntax).

<dl>
  <dt>rrb_source String</dt>
  <dd>Source code written in .rrb(Resticted Ruby) syntax.</dd>
</dl>

```ruby
scene "title" do
  color :palette_x1, :deepred, :darker
  paint range("44px", "5px", "120px", "3px"), :palette_x # range takes String parameter

  color :palette_y2, :pink, :darkest
  paint range(30, 2, 30, 12), :palette_y # range method can take Number as well. In this case x_from and x_to takes the value between 0 and 31, while y_from and y_to takes the value from 0 and 29.
end
```

### Sound

TBD

### Music

TBD

### Declare

TBD

## Burning Fuel DSL In Action

### Programming With .rrb(Restricted Ruby) Syntax

TBD(articles about #show, #sprite, #rand and #is_pressed are coming very soon)

## Notes

### License

GPLv3

### ToDos

* New VM Support
    * compatiblize with enchant.js
* Enhancement of Fuel DSL
    * for Screen, support screen scroll and simple sprite
    * for Screen, adding .bmp and .png support to make designing pattern table easier
    * for Sound, add triangle wave and noise effect
    * for Music, add custom instrument DSL
    * for Declare, support string and boolean declaration(currently only number and pattern table is supported)
* Improvement of Internal Architecture
    * make cc65 alternative in Ruby
* Other Feature To Be Supported
    * make burn rubygem work with mruby(not soon)
* Fix bugs
    * declaring 2x2 pattern works, however 2x1 pattern doesn't