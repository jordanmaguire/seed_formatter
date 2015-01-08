# IMPORTANT: SeedFormatter is deprecated

All,

I created SeedFormatter a long time ago, and it suited my needs at the time. I've since established a better interface and an expanded set of responsibilities. To facilitate this, I've created the [SeedHelper](https://github.com/jordanmaguire/seed_helper) gem that can be used instead. It has all the same functionality as SeedFormatter, and some more functionality on top of that.

[Check it out](https://github.com/jordanmaguire/seed_helper).

# SeedFormatter

SeedFormatter is a small gem I created while I was working on various projects with terrible, terrible Seed files.

## Purpose

The goal of SeedFormatter is to provide some commonly required functionality to be used in Seed files.

## Dependencies

SeedFormatter uses the [colored](https://github.com/defunkt/colored) gem to present output.

## Example: Seed file layout

I like my seed files to follow this format:

```ruby
class Seed
  class << self
    def create_categories
      # create some categories here
    end

    def create_admin_user
      # create an admin user here
    end
  end
end

Seed.create_categories
Seed.create_admin_user
```

I find this to be the neatest way to write Seed files.

## Example: Output

SeedFormatter provides four methods of showing output:

- `message` A general purpose message, generally used as a header. White by default.
- `success` Indicates a seed function was successful. Green by default.
- `error` Indicates a seed function has failed. Red by default.
- `spacer` Just to add a single space to separate groups of functionality.

Using these functions is straight forward (if we use the above example Seed file layout):

```ruby
class Seed
  class << self
    include SeedFormatter

    def create_categories
      message "Creating some categories"

      ["Sedan", "SUV", "Hatchback"].each do |category_name|
        category = Category.create :name => category_name
        if category.valid?
          success "Created #{category_name}"
        else
          error "Failed to create #{category_name}. Errors: #{category.errors.full_messages}"
        end
      end

      spacer
    end
  end
end
```

## Example: Overriding the display of output

You can override the display of output with the following options:

- `:prefix` Pass in a string to prefix the output.
- `:suffix` Pass in a string to suffix the output.
- `:color` Pass in a symbol representing the color your want the ouput to be. The list of acceptable colors can be found in colored gem's [documentation](https://github.com/defunkt/colored/blob/master/lib/colored.rb).

So, there are three methods of customizing the output of SeedFormatter.

1: Pass in the options

```ruby
success "I did it…they're all trees", {:prefix => "!!!", :suffix => "'''", :color => :magenta}
```

Which will yield the string `!!!I did it…they're all trees'''` in magenta.

2: Override the default function in your file or similarly,

3: Write your own

```ruby
def success message, options={}
  options[:prefix] ||= "!!!"
  options[:suffix] ||= "'''"
  options[:color]  ||= :magenta
  output message, options
end

def my_custom_output message, options={}
  options[:prefix] ||= "> "
  options[:suffix] ||= " <"
  options[:color]  ||= :yellow
  output message, options
end
```
