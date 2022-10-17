default: help

MAKEFLAGS += --warn-undefined-variables \
	     --no-builtin-rules \
	     --no-builtin-variables \
	     --no-print-directory
h:=\#
schemastore:=../../json
ALL:=

include Makefile.ALL
include Makefile.schemastore

%.cue: CUE_PACKAGE=$(notdir $(@D))# the single directory component directly containing the CUE file, with no trailing slash
%.cue: BASENAME=$(notdir $(@:.cue=))# the target filename with .cue suffix removed (NB this should be a version!)
%.cue: CUE_PATH=$(h)$(subst .,_,$(BASENAME:v%=V%)):# the version string with capitalised "V" prefix, and dots->underscores
%.cue:
	cue import jsonschema: $< --force --outfile $@ --package $(CUE_PACKAGE) --path '$(CUE_PATH)'

.PHONY: all
all: $(ALL) ## Create all out-of-date .cue schema files

.PHONY: clean
clean: ## Remove all .cue schema files, and their containing directories
	@rm -vf $(ALL)
	@rmdir -vp $(sort $(dir $(ALL))) || true

Makefile.ALL: Makefile.schemastore ## Automatically update the ALL list of .cue targets
	@cat $< | cut -f1 -d: | sed 's/^/ALL += /' >$@

help: ## Show this help
	@egrep -h '\s##\s' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'
