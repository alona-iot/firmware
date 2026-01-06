# Firmware Repository Structure

This document describes the planned filesystem structure for the firmware repository.

## Directory Tree

```
firmware/
  README.md
  LICENSE
  .gitignore
  .editorconfig

  docs/
    overview.md
    boards.md
    sensors.md
    networking.md
    testing.md
    troubleshooting.md

  protocol/                      # pinned schema/contracts from protocol repo (vendored)
    README.md
    VERSION
    mqtt/
      topics.md
    schemas/
      telemetry.schema.json
      event.schema.json
      heartbeat.schema.json

  tools/
    flash.sh
    monitor.sh
    ota.sh
    erase.sh
    gen_build_info.sh
    gen_secrets_template.sh
    ci_local.sh

  scripts/
    make_node_id.py
    validate_payloads.py
    pack_release.py

  sdkconfig.defaults             # sane baseline defaults (ESP-IDF)
  sdkconfig.release.defaults
  sdkconfig.debug.defaults

  managed_components/            # (if using ESP-IDF component manager) pinned deps

  components/                    # reusable modules across all nodes
    alona_core/                  # core primitives used everywhere
      CMakeLists.txt
      include/
        alona/log.h
        alona/time.h
        alona/ringbuf.h
        alona/metrics.h
      src/
        log.c
        time.c
        ringbuf.c
        metrics.c

    alona_config/
      CMakeLists.txt
      include/
        alona/config.h            # runtime config model, validation
      src/
        config.c

    alona_storage/
      CMakeLists.txt
      include/
        alona/nvs_kv.h
        alona/spool.h             # offline spool/buffer to flash
      src/
        nvs_kv.c
        spool.c

    alona_net/
      CMakeLists.txt
      include/
        alona/wifi.h
        alona/mqtt.h
        alona/ota.h
        alona/backoff.h
      src/
        wifi.c
        mqtt.c
        ota.c
        backoff.c

    alona_protocol/
      CMakeLists.txt
      include/
        alona/protocol.h          # encode/decode wrappers, version stamp
        alona/topic.h
      src/
        protocol.c
        topic.c

    alona_sensors/
      CMakeLists.txt
      include/
        alona/sensor.h            # generic sensor interface
        alona/sensor_bus_i2c.h
        alona/sensor_bus_1wire.h
        alona/sensor_bus_adc.h
      src/
        sensor.c
        sensor_bus_i2c.c
        sensor_bus_1wire.c
        sensor_bus_adc.c

    drivers/                      # chip-specific drivers (thin, testable)
      CMakeLists.txt
      include/
        drivers/ds18b20.h
        drivers/bme280.h
        drivers/hx711.h
        drivers/ina219.h
        drivers/ads1115.h
        drivers/jsn_sr04t.h
      src/
        ds18b20.c
        bme280.c
        hx711.c
        ina219.c
        ads1115.c
        jsn_sr04t.c

  main/                           # default app entry (can host a "reference node")
    CMakeLists.txt
    app_main.c

  nodes/                          # each node is an app that composes components+drivers
    node-template/
      README.md
      CMakeLists.txt
      main/
        app_main.c
        node_config.h
        node_wiring.h             # pin map
        node_tasks.c              # sampling loop, publish loop, heartbeat loop
      partitions.csv
      sdkconfig.defaults

    well-level/
      README.md
      CMakeLists.txt
      main/
        app_main.c
        node_config.h
        node_wiring.h
        sensors.c                 # water level sensor integration
        publish.c
      partitions.csv
      sdkconfig.defaults

    energy-meter/
      README.md
      CMakeLists.txt
      main/
        app_main.c
        node_config.h
        node_wiring.h
        sensors.c
        publish.c

    environment/
      README.md
      CMakeLists.txt
      main/
        app_main.c
        node_config.h
        node_wiring.h
        sensors.c
        publish.c

  test/
    README.md
    host/                         # host-based unit tests (where possible)
      test_backoff.c
      test_topic.c
      test_spool.c

  ci/
    github/
      workflows/
        build.yml
        lint.yml
        release.yml

  releases/
    README.md
    manifests/
      firmware-manifest.schema.json
```
