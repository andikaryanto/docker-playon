FROM varnish:7.5-alpine

EXPOSE 80

CMD ["varnishd", "-f", "/etc/varnish/default.vcl", "-a", ":80"]