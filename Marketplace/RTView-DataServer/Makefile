TAG ?= 4.2.0.0

# crd.Makefile provides targets to install Application CRD.
include ../Marketplace-Tools/crd.Makefile

# gcloud.Makefile provides default values for
# REGISTRY and NAMESPACE derived from local
# gcloud and kubectl environments.
include ../Marketplace-Tools/gcloud.Makefile

# marketplace.Makefile provides targets such as
# ".build/marketplace/deployer/envsubst" to build the base
# deployer images locally.
include ../Marketplace-Tools/marketplace.Makefile

# ubbagent.Makefile provides ".build/ubbagent/ubbagent"
# target to build the ubbagent image locally.
include ../Marketplace-Tools/ubbagent.Makefile
include ../Marketplace-Tools/var.Makefile

# app.Makefile provides the main targets for installing the
# application.
# It requires several APP_* variables defined as followed.
include ../Marketplace-Tools/app.Makefile

APP_DEPLOYER_IMAGE ?= $(REGISTRY)/deployer:$(TAG)
NAME ?= RTView-DataServer-1
RTV_USERNAME ?= demo
RTV_PASSWORD ?= demo
APP_PARAMETERS ?= { \
	"name": "$(NAME)", \
	"namespace": "$(NAMESPACE)", \
	"rtviewDataserverImage": "$(REGISTRY)/RTView-DataServer:$(TAG)", \
	"rtviewHistorianImage": "$(REGISTRY)/RTView-DataServer-historian:$(TAG)", \
	"rtviewHistoryDbImage": "$(REGISTRY)/RTView-DataServer-history-db:$(TAG)", \
	"ubbAgentImage": "$(REGISTRY)/ubbagent:$(TAG)", \
	"global.image.variant": "$(VARIANT)", \
	"rtview-security-proxy.security.username": "$(RTV_USERNAME)", \
	"rtview-security-proxy.security.password": "$(RTV_PASSWORD)" \
}

# Extend the target as defined in app.Makefile to
# include real dependencies.
app/build-images:: .build/RTView-DataServer/rtview-dataserver \
										.build/RTView-DataServer/ubbagent \
										.build/RTView-DataServer/deployer


.build/RTView-DataServer: | .build
	mkdir -p "$@"

.build/RTView-DataServer/deployer: deployer/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-dataserver/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-dataserver/templates/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-historian/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-historian/templates/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-history-db/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-history-db/templates/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-security-proxy/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/charts/rtview-security-proxy/templates/* \
																	 ../RTView-Kubernetes/RTView-DataServer/chart/templates/* \
																	 schema.yaml \
																	 .build/marketplace/deployer/helm \
																	 .build/var/APP_DEPLOYER_IMAGE \
																	 .build/var/REGISTRY \
																	 .build/var/TAG \
																	 | .build/RTView-DataServer
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
.build/RTView-DataServer/rtview-dataserver: .build/var/REGISTRY \
																						.build/var/TAG \
																						../RTView-Kubernetes/RTView-DataServer/scripts/build-dataserver.sh \
																						| .build/RTView-DataServer
	$(call print_target, $@)
	chmod +x ../RTView-Kubernetes/RTView-DataServer/scripts/build-dataserver.sh
	REGISTRY="$(REGISTRY)" ../RTView-Kubernetes/RTView-DataServer/scripts/build-dataserver.sh "$(TAG)"
	@touch "$@"


.build/RTView-DataServer/ubbagent: .build/ubbagent/ubbagent \
																	 .build/var/REGISTRY \
																	 .build/var/TAG \
																	 | .build/RTView-DataServer
	$(call print_target, $@)
	docker build \
		--tag "$(REGISTRY)/ubbagent:$(TAG)" \
		-f ubbAgent/Dockerfile \
		ubbAgent/
	docker push "$(REGISTRY)/ubbagent:$(TAG)"
	@touch "$@"
