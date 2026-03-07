# dekube-rewriter-nginx

![vibe coded](https://img.shields.io/badge/vibe-coded-ff69b4)
![python 3](https://img.shields.io/badge/python-3-3776AB)
![heresy: 1/10](https://img.shields.io/badge/heresy-1%2F10-brightgreen)
![stdlib only](https://img.shields.io/badge/dependencies-stdlib%20only-brightgreen)
![public domain](https://img.shields.io/badge/license-public%20domain-brightgreen)

Nginx ingress annotation rewriter for [dekube](https://dekube.io).

Excluded from the core because nginx-ingress is deprecated. Still here, because legacy doesn't care about deprecation notices — and neither does your helmfile.

## Handled annotations

| Annotation | Entry field |
|------------|-------------|
| `nginx.ingress.kubernetes.io/rewrite-target: /$1` | `strip_prefix` |
| `nginx.ingress.kubernetes.io/backend-protocol: HTTPS` | `scheme: https` |
| `nginx.ingress.kubernetes.io/enable-cors: "true"` | `response_headers` (CORS headers) |
| `nginx.ingress.kubernetes.io/cors-allow-origin` | `response_headers["Access-Control-Allow-Origin"]` |
| `nginx.ingress.kubernetes.io/cors-allow-methods` | `response_headers["Access-Control-Allow-Methods"]` |
| `nginx.ingress.kubernetes.io/cors-allow-headers` | `response_headers["Access-Control-Allow-Headers"]` |
| `nginx.ingress.kubernetes.io/proxy-body-size` | `max_body_size` |
| `nginx.ingress.kubernetes.io/configuration-snippet` | Partial: `more_set_headers` extracted to `response_headers` |

## Matching

Matches Ingress manifests with:

- `ingressClassName: nginx` (or mapped via `ingressTypes` in config)
- Any `nginx.ingress.kubernetes.io/*` annotation

## Installation

```bash
python3 dekube-manager.py nginx
```

Or manually:

```bash
cp nginx_rewriter.py /path/to/extensions-dir/
```

## Code quality

*Last updated: 2026-03-07*

| Metric | Value |
|--------|-------|
| Pylint | 9.64/10 |
| Pyflakes | clean |
| Radon MI | 56.40 (A) |
| Radon avg CC | 15.5 (C) |

Worst CC: `NginxRewriter.rewrite` (18, C).

The `E0401: Unable to import 'dekube'` is expected — extensions import from dekube-engine at runtime, not at lint time.

## Dependencies

None (stdlib only).
