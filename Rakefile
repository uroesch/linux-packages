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

def create_hashfile(file)
  directory = File.dirname(file) 
  basename  = File.basename(file)
  cd directory do
    sh %(md5sum #{basename} > #{basename}.md5)
    sh %(sha256sum #{basename} > #{basename}.sha256)
  end
end


task :default => :dist_packages

desc "Distrubute packages into sub directories"
task :dist_packages do
  FILES.each do |file|
    subdir = determine_subdir(file)
    target = File.join(subdir, File.basename(file))
    mkdir subdir unless File.exist?(subdir)
    begin
      puts target
      unless File.exist?(target) 
        cp file, target 
        create_hashfile(target)
        sh %(git add #{target}*)
      end
    ensure
      rm file 
    end
  end
end
