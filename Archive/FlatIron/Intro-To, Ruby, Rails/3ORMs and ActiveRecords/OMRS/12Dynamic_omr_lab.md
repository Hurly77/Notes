```ruby
class Student < InteractiveRecord
#THIS IS THE CHILD CLASS
self.column_names.each do |col_name|
attr_accessor col_name.to_sym
  end
  
end
```

**self.class.foo vs. self.foo **-- **self.foo** is used when your in a **class method**; **self.class.foo** is so the **instance** knows you operation on the **class**

**send**- the send method "sends" the information **to** the instance of an **object**

when do you use quotes on interpolation? we use the **(' ')** so that if an integer it can take in numbers and strings

```ruby
class InteractiveRecord 

    
    
  def self.table_name
    self.to_s.downcase.pluralize
  end
  
  def self.column_names
     DB[:conn].results_as_hash = true
     sql = "PRAGMA table_info('#{table_name}')"
     table_info = DB[:conn].execute(sql)
     column_names = []
     table_info.each do |column|
     column_names << column["name"]
    end
    column_names.compact
  end
  
    def initialize(options={})
    options.each do |property, value|
      self.send("#{property}=", value)
    end
  end

  def table_name_for_insert
    self.class.table_name
  end

  def col_names_for_insert
    self.class.column_names.delete_if {|col| col == "id"}.join(", ")
  end
  
  def values_for_insert
    values = []
    self.class.column_names.each do |col_name|
    values << "'#{send(col_name)}'" unless send(col_name).nil?
    end
    values.join(", ")
  end
  
  def save
    sql = "INSERT INTO #{table_name_for_insert} (#{col_names_for_insert}) 
    VALUES (#{values_for_insert})"
    DB[:conn].execute(sql)
    @id = DB[:conn].execute("SELECT last_insert_rowid() 
    FROM #{table_name_for_insert}")[0][0]
  end
#AT THIS POINT I HAD TO RUN BUNDLE INSTALL
  def self.find_by_name(name)
  DB[:conn].execute("SELECT * FROM #{self.table_name} 
  WHERE name = ?", [name])
  end

  def self.find_by(h) #made my argument h for hash
    column = h.keys[0].to_s #set a variable = h Wich is a hash with only on key, and .keys to grab [0] index wich is the only key and used .to_S to make it a string
    value = h.values[0]# here I did the same as but I did't change it to a string

    DB[:conn].execute("SELECT * FROM #{self.table_name} #SELECT ALL FROM TABLE 
    WHERE #{column} = '#{value}'")#
      where my column is = to my value
  end
end
```

