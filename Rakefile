## Rake file to put packages into subdirectories

PKG_EXT         = %w( deb rpm )
KEYWORDS        = %w( rubygem )
KEYWORDS_REGEXP = %r{^(#{Regexp.union(KEYWORDS)})}
FILES           = Rake::FileList.new.include(*PKG_EXT.map { |x| '*/*.' << x })

def determine_subdir(file) 
  basename = File.basename(file)
  dirname  = File.dirname(file)
  File.join(dirname, subdir(basename))
end

def subdir(basename)
  case
  when basename.match(KEYWORDS_REGEXP) 
    $1
  else 
    basename[0]
  end
end
 

task :default => :dist_packages

task :dist_packages do 
  FILES.each do |file|
    subdir = determine_subdir(file)
    mkdir subdir unless File.exist?(subdir)
    mv file, subdir
  end
end
