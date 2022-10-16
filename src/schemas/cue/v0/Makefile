MAKEFLAGS += --warn-undefined-variables \
	     --no-builtin-rules \
	     --no-builtin-variables \
	     --no-print-directory

DEFAULT: help

ALL:=# the contents of ALL is defined in the included Makefile.all_targets, which is remade on each invocation
include Makefile.all_targets

.PHONY: all
all: $(ALL) ## Create all out-of-date .cue schema files

.PHONY: clean
clean: ## Remove all .cue schema files, and their containing directories
	@rm -vf $(ALL)
	@rmdir -vp $(sort $(dir $(ALL))) || true

j:=../../json

h:=\#
%.cue: CUE_PACKAGE=$(notdir $(@D))# the single directory component directly containing the CUE file, with no trailing slash
%.cue: BASENAME=$(notdir $(@:.cue=))# the target filename with .cue suffix removed (NB this should be a version!)
%.cue: CUE_PATH=$(h)$(subst .,_,$(BASENAME:v%=V%)):# the version string with capitalised "V" prefix, and dots->underscores
%.cue:
	cue import jsonschema: $< --force --outfile $@ --package $(CUE_PACKAGE) --path '$(CUE_PATH)'

Makefile.all_targets: Makefile ## Automatically update the list of all .cue targets, poorly
	@grep '^[a-zA-Z][a-zA-Z0-9/_.]*\.cue:' Makefile | cut -f1 -d: | sed 's/^/ALL += /' >Makefile.all_targets
help: ## Show this help
	@egrep -h '\s##\s' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'
