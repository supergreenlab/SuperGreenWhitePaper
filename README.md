![SuperGreenLab](assets/sgl.png?raw=true "SuperGreenLab")

Table of Contents
=================

   * [Introduction](#introduction)
   * [SuperGreenLab](#supergreenlab)
      * [Controller hardware](#controller-hardware)
      * [Controller firmware](#controller-firmware)
      * [Controller app](#controller-app)
      * [Backend](#backend)

# Introduction

This document is intended to be the reference guide and manual, intended for those who want to join us.

# SuperGreenLab

There are 3 main components:

## Controller hardware

The controller hardware is in charge of what is mandatory to grow, nothing more. As a rule of thumb, if the plant can grow without the feature, we're not doing it.
Extra hardwares will be created for those that want that extra mile. Please see the backend section to know how you could add more data from any (custom or not) devices.

Right now we identified these features as mandatory:

- Reliable light scheduling
- Powerful controllable ventilation, with possibility of automatic temperature / humidity adjustment
- Control each light channels independantly so you can control the stretch factor of the plant (if the lights are too close, it'll just stop growing taller, but will thicken, you want something in-between)
- Compatible with all sorts of sensors
- Remote control, and remote monitoring
- Compatible with already existing lightning systems (limitations exist)

## Controller firmware

The firmware is here to be more generic on the other hand.
So while the main repository is at https://github.com/supergreenlab/SuperGreenOS, it is actually based on https://github.com/supergreenlab/SuperGreenOSBoilerplate.

So while SuperGreenOS is dedicated to guaranty the features listed above (See Controller Hardware), SuperGreenOSBoilerplate can be a good starting point for any other esp32 based devices, especially if you plan to have the device's data "mix" with the controller's datas.
For example: auto-fertigation system that would stream it's data on the app's interface.

Apart from the features listed above (See Controller Hardware), there are a few other features that come as mandatory:

- Keep a wifi connection as much as possible
- OTA (Over-The-Air) updates, to keep up with new features and bug fixes
- Various means of control, right now only http (over wifi AP or STA) and ble as a backup
- Anti brickage features, if you mess up with the keys, it should be able to recover in a way or another, even if it means resetting all keys to defaults.
- Sink all data to the backend for alerts and monitoring, and gives easy means to add more data.
- Give full control, even if the desktop app imposes limitations to avoid breaking stuff inadvertently, the firmware gives access to all possible configurations over http or other mean.

## Controller app

The controller app is the main "user-friendly" interface, it's feature are meant to be limited to "normal" usage of the box:

- Initial setup
- Start/stop a given box
- Switch from veg to bloom
- Control ventilation power
- Configure lights maximum intensity
- View live data, graphs and live views
- History of alerts (temp/humidity)
- Send health reports (Heap memory is logged for example)

For now, it is available as a standalone Desktop application (Windows, Mac, Linux), and as a web app at http://app.supergreenlab.com

## Backend

The backend only puprose right now, is to receive and send back datas from the devices for further analysis or just display (you're not storing days of data in the esp32's flash memory).

On the user's side, there is:
- MQTT server who's port is reachable from the exterior
- And a http server that allows to retrieve sent data from the device's unique identifier (mac address)

On the developper's side, there is:
- Prometheus for time series storage and processing
- AlertManager: plugs to Prometheus and produces alerts based on timeseries values
- Redis server which stores all variable sent by the device, to keep track of last values set
- Grafana: displays Prometheus's timeseries in dashboards

There is currently no mean to actually identify a user, the only link that exists is the unique identifier of your device, that acts as an auth token. 

The plan is to keep the backend as easy to setup your own as possible, even if we have to break this plan, we'll keep a light-weight version available.
