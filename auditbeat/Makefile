BEAT_NAME=auditbeat
BEAT_DESCRIPTION=Audit the activities of users and processes on your system.
SYSTEM_TESTS=false
TEST_ENVIRONMENT=false

# Path to the libbeat Makefile
-include ../libbeat/scripts/Makefile

# This is called by the beats packer before building starts
.PHONY: before-build
before-build:

# Collects all dependencies and then calls update
.PHONY: collect
collect: fields collect-docs configs kibana

# Collects all module and metricset fields
.PHONY: fields
fields: python-env
	@mkdir -p _meta
	@cp ${ES_BEATS}/metricbeat/_meta/fields.common.yml _meta/fields.generated.yml
	@${PYTHON_ENV}/bin/python ${ES_BEATS}/metricbeat/scripts/fields_collector.py >> _meta/fields.generated.yml

# Collects all module configs
.PHONY: configs
configs: python-env
	@cp ${ES_BEATS}/auditbeat/_meta/common.yml _meta/beat.yml
	@${PYTHON_ENV}/bin/python ${ES_BEATS}/script/config_collector.py --beat ${BEAT_NAME} $(PWD) >> _meta/beat.yml
	@cp ${ES_BEATS}/auditbeat/_meta/common.reference.yml _meta/beat.reference.yml
	@${PYTHON_ENV}/bin/python ${ES_BEATS}/script/config_collector.py --beat ${BEAT_NAME} --full $(PWD) >> _meta/beat.reference.yml

# Collects all module docs
.PHONY: collect-docs
collect-docs: python-env
	@-rm -rf docs/modules
	@mkdir -p docs/modules
	@${PYTHON_ENV}/bin/python ${ES_BEATS}/metricbeat/scripts/docs_collector.py --beat ${BEAT_NAME}

# Collects all module dashboards
.PHONY: kibana
kibana:
	@-rm -rf _meta/kibana/dashboard _meta/kibana/search _meta/kibana/visualization # Skip index-pattern
	@mkdir -p _meta/kibana
	@-cp -pr module/*/_meta/kibana _meta/

