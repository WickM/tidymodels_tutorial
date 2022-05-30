source("renv/activate.R")

library(httr)
library(curl)

httr::reset_config()
set_config(config = use_proxy(username = Sys.getenv("igc_api_user"),
password = Sys.getenv("igc_api_pw"), url = "proxy.wuestenrot.at:3128"))

new_handle_plain = curl::new_handle

new_handle_ntlm = function(){
  handle = new_handle_plain()
  handle_setopt(handle, .list = list(PROXYUSERPWD = glue("{Sys.getenv('igc_api_user')}:{Sys.getenv('igc_api_pw')}")))
  return(handle)
}

rlang::env_unlock(env = asNamespace('curl'))
rlang::env_binding_unlock(env = asNamespace('curl'))
assign('new_handle', new_handle_ntlm, envir = asNamespace('curl'))
rlang::env_binding_lock(env = asNamespace('curl'))
rlang::env_lock(asNamespace('curl'))

Sys.setenv("http_proxy" = curl::ie_get_proxy_for_url("http://orf.at"))
Sys.setenv("https_proxy" = curl::ie_get_proxy_for_url("http://orf.at"))
