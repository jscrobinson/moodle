

# http://clarkgrubb.com/makefile-style-guide



TERRAFORM := terraform

STACK=school
COLOUR=longden
ENVNAME=production
REGION=eu-west-2


.PHONY: check

check:

ifndef TERRAFORM

    $(error "terraform 0.12 is not available please install terraform")

endif



MAKEFLAGS += --warn-undefined-variables --no-print-directory

SHELL := bash

.SHELLFLAGS := -eu -o pipefail -c

.DEFAULT_GOAL := plan

.DELETE_ON_ERROR:

.SUFFIXES:



REGION?=eu-west-2

ENVNAME?=production

TFSTATE_BUCKET?=jimfrastructure-terraform-$(REGION)-$(ENVNAME)-state-store

COLOUR?=longden



export AWS_DEFAULT_REGION=$(REGION)



.PHONY: begin

begin: plan



.PHONY: get_modules

get_modules:

	cd ../modules && pterrafile



.PHONY: clean
clean: check
	rm -rf .terraform/terraform.tfstate

.PHONY: init
init: clean
	${TERRAFORM} --version
	${TERRAFORM} init \
		-backend-config="bucket=$(TFSTATE_BUCKET)" \
		-backend-config="key=$(COLOUR)/$(STACK).tfstate" \
		-backend-config="region=$(REGION)"



.PHONY: plan
plan: init
	${TERRAFORM} plan -var-file=params/$(REGION)/$(ENVNAME)/$(COLOUR).tfvars .

.PHONY: apply
apply: init
	${TERRAFORM} apply -var-file=params/$(REGION)/$(ENVNAME)/$(COLOUR).tfvars .

.PHONY: output
output: init
	${TERRAFORM} output

.PHONY: destroy
destroy: init
	${TERRAFORM} plan -destroy -var-file=params/$(REGION)/$(ENVNAME)/$(COLOUR).tfvars .
	${TERRAFORM} destroy -var-file=params/$(REGION)/$(ENVNAME)/$(COLOUR).tfvars .

