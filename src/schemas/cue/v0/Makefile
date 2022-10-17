default: help

MAKEFLAGS += --warn-undefined-variables \
	     --no-builtin-rules \
	     --no-builtin-variables \
	     --no-print-directory
h:=\#
VPATH=../../json
ALL:=

include Makefile.ALL
include Makefile.schemastore

pascal_case = $(shell echo '$(1)' | tr -c 'a-zA-Z0-9' ' ' | sed -E 's/\<./\U&/g' | tr -d ' ')

%.cue: BASENAME=$(notdir $(@:.cue=))# the target filename with .cue suffix removed (NB this should be a version!)
%.cue: LEAF_DIR_PASCAL=$(call pascal_case,$(notdir $(@D)))
%.cue: CUE_PATH=$(h)$(LEAF_DIR_PASCAL):
%.cue: CUE_PACKAGE=$(subst .,_,$(BASENAME))
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
