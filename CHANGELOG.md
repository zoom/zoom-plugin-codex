# Changelog

All notable changes to this plugin are documented in this file.

## 1.2.1

- synced the canonical Marketplace template catalog, including the expanded MCP registration manifests
- added the machine-readable template index and safe full-replacement workflow for updating existing General App manifests
- updated skill and command routing to distinguish General App manifests from native S2S and Meeting SDK create requests
- documented the marketplace-helper MCP endpoint and clarified that created apps must use each user's own OAuth, home, and webhook endpoints
- bundled the production marketplace-helper MCP connection and OAuth scopes with the plugin

## Unreleased

## 1.2.0

- documented the seven official Zoom-hosted MCP server surfaces, endpoints, tools, scope families, and matching Marketplace templates
- added ngrok and Cloudflare Tunnel guidance for testing Marketplace-created apps without a deployed HTTPS server

## 1.1.0

- added `/setup-zoom-marketplace-app` and the matching skill for app-model selection, manifest validation, scopes, events, credential handling, and post-create setup
- bundled 25 scenario templates for General, S2S, Meeting SDK, webhook, WebSocket, RTMS, Team Chat, Phone, Contact Center, Zoom Apps, Plugin SDK, and MCP registration paths
- updated planning, OAuth, build, event, RTMS, and integration-doctor commands to verify the owning Marketplace or Build Platform app before implementation
- clarified Meeting/Webinar RTMS, Contact Center Voice RTMS, Team Chat chatbot, and WebSocket app-model requirements

## 1.0.0

- consolidated the Zoom developer workflows into the app-backed `Zoom` plugin
- added the Zoom build, setup, planning, debugging, skill, command, and reviewer-agent workflows
- kept the Zoom app mapping, screenshots, and sideload metadata in this repo
