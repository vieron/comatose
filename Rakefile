require "css_parser"

namespace :compile do
  task :css do
    
    if ENV.include? "path"
      path = ENV["path"]
    else
      raise "\nmust specify a path. ex. rake compile:css path=path_to_file.css"
    end
    
    if ENV.include? "output"
      output_path = ENV["output"]
    else
      raise "\nmust specify an output path. ex. rake compile:css output_path=path_to_compiled_file.css"
    end
    
    parser = CssParser::Parser.new
    parser.load_file!(path)
    
    styles = {}
    
    parser.each_rule_set do |rule_set|
      rule_set.selectors.each do |selector|
        rule_set.each_declaration do |property, value, is_important|
          $property = styles[property] ||= {}
          $value = $property[value] ||= {:normal=>[],:important=>[]}
          $style = is_important ? $value[:important] : $value[:normal]
          unless $style.include? selector
            $style << selector
          end
          # puts %~#{property} <<< #{value} <<< #{is_important}~
        end
      end
    end
    
    output = ""
    
    styles.each do |property_key,property_value|
      property_value.each do |style_value_key, style_values|
        unless style_values[:normal].empty?
          output += style_values[:normal].join(",") 
          output += %~
{
  #{property_key}:#{style_value_key};
}

~
        end
        unless style_values[:important].empty?
          output += style_values[:important].join(",") 
          ouput += %~{
  #{property_key}:#{style_value_key};
}

~
        end
      end
    end
    
    File.delete(output_path) if File.exists?(output_path)
    File.open(output_path, 'w') {|f| f.write(output) }
    # puts output.to_s
    
  end
end