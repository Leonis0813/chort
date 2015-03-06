FILES=`ls markdown | cut -d "." -f 1`

all: doc

doc:
	rm sphinx/*.rst
	@FILES="$(FILES)";\
	for name in $$FILES; do\
		echo "pandoc -f markdown -t rst -o sphinx/$${name}.rst markdown/$${name}.md";\
		pandoc -f markdown -t rst -o sphinx/$${name}.rst markdown/$${name}.md;\
	done
	cd sphinx; make html
