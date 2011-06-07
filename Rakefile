name="doc" # if you change this you might want to edit .gitignore too

latex="pdflatex --interaction batchmode"
file "#{name}.fls" => "#{name}.tex" do |t|
        sh "#{latex} -recorder #{name}"
end
unless (File.exist? "#{name}.fls") 
        sh "#{latex} -recorder #{name}"
        rm "#{name}.pdf"
end
deps = File.open("#{name}.fls").to_a.select{|i| i.start_with? "INPUT"}.map{|i| i.split(" ")[1].chomp} + ["bib.bib"]
file "#{name}.pdf" => deps do |t|
	sh "#{latex} #{name}"
	sh "grep -q citation #{name}.aux && bibtex #{name} || echo skipping bibtex" #only run bibtex if needed
	sh "#{latex} #{name}"
	sh "#{latex} -recorder #{name}"
end

task :view => "#{name}.pdf" do |t|
        sh "open #{name}.pdf"
end

file "bib.bib" => ["basic.bib","extra.bib"] do |t|
	sh "cat #{t.prerequisites.join(" ")} > #{t.name}"
end

task :all => "#{name}.pdf" do |t|
end

task :default => :all

task :clean  do |t|
        ["bbl","fls","pdf","log","aux","blg"].each do |ext|
                rm name+"."+ext , :force => true
        end
        rm "bib.bib"
end
