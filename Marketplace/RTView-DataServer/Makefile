TAG ?= 4.2.0.0

# crd.Makefile provides targets to install Application CRD.
include ./tools/crd.Makefile

# gcloud.Makefile provides default values for
# REGISTRY and NAMESPACE derived from local
# gcloud and kubectl environments.
include ./tools/gcloud.Makefile

# marketplace.Makefile provides targets such as
# ".build/marketplace/deployer/envsubst" to build the base
# deployer images locally.
include ./tools/marketplace.Makefile

# ubbagent.Makefile provides ".build/ubbagent/ubbagent"
# target to build the ubbagent image locally.
include ./tools/ubbagent.Makefile
include ./tools/var.Makefile

# app.Makefile provides the main targets for installing the
# application.
# It requires several APP_* variables defined as followed.
include ./tools/app.Makefile

APP_DEPLOYER_IMAGE ?= $(REGISTRY)/deployer:$(TAG)
NAME ?= rtview-dataserver-1
RTV_USERNAME ?= demo
RTV_PASSWORD ?= demo
APP_PARAMETERS ?= { \
	"name": "$(NAME)", \
	"namespace": "$(NAMESPACE)", \
	"rtviewDataserverImage": "$(REGISTRY)/rtview-dataserver:$(TAG)", \
	"rtviewHistorianImage": "$(REGISTRY)/rtview-dataserver-historian:$(TAG)", \
	"rtviewHistoryDbImage": "$(REGISTRY)/rtview-dataserver-history-db:$(TAG)", \
	"ubbAgentImage": "$(REGISTRY)/ubbagent:$(TAG)", \
	"global.image.variant": "$(VARIANT)", \
	"rtview-security-proxy.security.username": "$(RTV_USERNAME)", \
	"rtview-security-proxy.security.password": "$(RTV_PASSWORD)" \
}

# Extend the target as defined in app.Makefile to
# include real dependencies.
app/build-images:: .build/rtview-dataserver/rtview-dataserver \
										.build/rtview-dataserver/ubbagent \
										.build/rtview-dataserver/deployer


.build/rtview-dataserver: | .build
	mkdir -p "$@"

.build/rtview-dataserver/deployer: deployer/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-dataserver/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-dataserver/templates/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-historian/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-historian/templates/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-history-db/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-history-db/templates/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-security-proxy/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/charts/rtview-security-proxy/templates/* \
																	 ../rtview-kubernetes/rtview-dataserver/chart/templates/* \
																	 schema.yaml \
																	 .build/marketplace/deployer/helm \
																	 .build/var/APP_DEPLOYER_IMAGE \
																	 .build/var/REGISTRY \
																	 .build/var/TAG \
																	 | .build/rtview-dataserver
	$(call print_target, $@)
	docker build \
			--build-arg REGISTRY="$(REGISTRY)" \
			--build-arg TAG="$(TAG)" \
			--tag "$(APP_DEPLOYER_IMAGE)" \
			-f deployer/Dockerfile \
			../
	docker push "$(APP_DEPLOYER_IMAGE)"
	@touch "$@"


# Simulate building of primary app image. Actually just copying public image to
# local registry.
.build/rtview-dataserver/rtview-dataserver: .build/var/REGISTRY \
																						.build/var/TAG \
																						../rtview-kubernetes/rtview-dataserver/scripts/build-dataserver.sh \
																						| .build/rtview-dataserver
	$(call print_target, $@)
	chmod +x ../rtview-kubernetes/rtview-dataserver/scripts/build-dataserver.sh
	REGISTRY="$(REGISTRY)" ../rtview-kubernetes/rtview-dataserver/scripts/build-dataserver.sh "$(TAG)"
	@touch "$@"


.build/rtview-dataserver/ubbagent: .build/ubbagent/ubbagent \
																	 .build/var/REGISTRY \
																	 .build/var/TAG \
																	 | .build/rtview-dataserver
	$(call print_target, $@)
	docker build \
		--tag "$(REGISTRY)/ubbagent:$(TAG)" \
		-f ubbAgent/Dockerfile \
		ubbAgent/
	docker push "$(REGISTRY)/ubbagent:$(TAG)"
	@touch "$@"
