cmake_minimum_required(VERSION 3.10)

set(CMAKE_VERBOSE_MAKEFILE OFF)

project(nginx)

set(CMAKE_C_COMPILER   "/usr/local/gcc/bin/gcc")
set(CMAKE_CXX_COMPILER   "/usr/local/gcc/bin/g++")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -pipe  -O -W -Wall -Wpointer-arith -Wno-unused-parameter -Werror -ggdb")

SET( ALL_INCS
  "src/core"
	"src/event"
	"src/event/modules"
	"src/os/unix"
	"objs"
	"src/http"
	"src/http/modules"
	"src/http/v2"
	"src/mail"
	"src/stream"
  )

# 	-ldl -lpthread -lcrypt -lpcre -lssl -lcrypto -ldl -lpthread -lz \
# 	-Wl,-E
SET(COMPI_OPT 
	-ldl -lpthread -lcrypt -lpcre -lssl -lcrypto -ldl -lpthread -lz -Wl,-E
  )


set( SOURCE_FILES
	"src/core/nginx.c"
	"src/core/ngx_log.c"
	"src/core/ngx_palloc.c"
	"src/core/ngx_array.c"
	"src/core/ngx_list.c"
	"src/core/ngx_hash.c"
	"src/core/ngx_buf.c"
	"src/core/ngx_queue.c"
	"src/core/ngx_output_chain.c"
	"src/core/ngx_string.c"
	"src/core/ngx_parse.c"
	"src/core/ngx_parse_time.c"
	"src/core/ngx_inet.c"
	"src/core/ngx_file.c"
	"src/core/ngx_crc32.c"
	"src/core/ngx_murmurhash.c"
	"src/core/ngx_md5.c"
	"src/core/ngx_sha1.c"
	"src/core/ngx_rbtree.c"
	"src/core/ngx_radix_tree.c"
	"src/core/ngx_slab.c"
	"src/core/ngx_times.c"
	"src/core/ngx_shmtx.c"
	"src/core/ngx_connection.c"
	"src/core/ngx_cycle.c"
	"src/core/ngx_spinlock.c"
	"src/core/ngx_rwlock.c"
	"src/core/ngx_cpuinfo.c"
	"src/core/ngx_conf_file.c"
	"src/core/ngx_module.c"
	"src/core/ngx_resolver.c"
	"src/core/ngx_open_file_cache.c"
	"src/core/ngx_crypt.c"
	"src/core/ngx_proxy_protocol.c"
	"src/core/ngx_syslog.c"
	"src/event/ngx_event.c"
	"src/event/ngx_event_timer.c"
	"src/event/ngx_event_posted.c"
	"src/event/ngx_event_accept.c"
	"src/event/ngx_event_udp.c"
	"src/event/ngx_event_connect.c"
	"src/event/ngx_event_pipe.c"
	"src/os/unix/ngx_time.c"
	"src/os/unix/ngx_errno.c"
	"src/os/unix/ngx_alloc.c"
	"src/os/unix/ngx_files.c"
	"src/os/unix/ngx_socket.c"
	"src/os/unix/ngx_recv.c"
	"src/os/unix/ngx_readv_chain.c"
	"src/os/unix/ngx_udp_recv.c"
	"src/os/unix/ngx_send.c"
	"src/os/unix/ngx_writev_chain.c"
	"src/os/unix/ngx_udp_send.c"
	"src/os/unix/ngx_udp_sendmsg_chain.c"
	"src/os/unix/ngx_channel.c"
	"src/os/unix/ngx_shmem.c"
	"src/os/unix/ngx_process.c"
	"src/os/unix/ngx_daemon.c"
	"src/os/unix/ngx_setaffinity.c"
	"src/os/unix/ngx_setproctitle.c"
	"src/os/unix/ngx_posix_init.c"
	"src/os/unix/ngx_user.c"
	"src/os/unix/ngx_dlopen.c"
	"src/os/unix/ngx_process_cycle.c"
	"src/os/unix/ngx_linux_init.c"
	"src/event/modules/ngx_epoll_module.c"
	"src/os/unix/ngx_linux_sendfile_chain.c"
	"src/event/ngx_event_openssl.c"
	"src/event/ngx_event_openssl_stapling.c"
	"src/core/ngx_regex.c"
	"src/http/ngx_http.c"
	"src/http/ngx_http_core_module.c"
	"src/http/ngx_http_special_response.c"
	"src/http/ngx_http_request.c"
	"src/http/ngx_http_parse.c"
	"src/http/modules/ngx_http_log_module.c"
	"src/http/ngx_http_request_body.c"
	"src/http/ngx_http_variables.c"
	"src/http/ngx_http_script.c"
	"src/http/ngx_http_upstream.c"
	"src/http/ngx_http_upstream_round_robin.c"
	"src/http/ngx_http_file_cache.c"
	"src/http/ngx_http_huff_decode.c"
	"src/http/ngx_http_huff_encode.c"
	"src/http/ngx_http_write_filter_module.c"
	"src/http/ngx_http_header_filter_module.c"
	"src/http/modules/ngx_http_chunked_filter_module.c"
	"src/http/v2/ngx_http_v2_filter_module.c"
	"src/http/modules/ngx_http_range_filter_module.c"
	"src/http/modules/ngx_http_gzip_filter_module.c"
	"src/http/ngx_http_postpone_filter_module.c"
	"src/http/modules/ngx_http_ssi_filter_module.c"
	"src/http/modules/ngx_http_charset_filter_module.c"
	"src/http/modules/ngx_http_sub_filter_module.c"
	"src/http/modules/ngx_http_userid_filter_module.c"
	"src/http/modules/ngx_http_headers_filter_module.c"
	"src/http/ngx_http_copy_filter_module.c"
	"src/http/modules/ngx_http_not_modified_filter_module.c"
	"src/http/v2/ngx_http_v2.c"
	"src/http/v2/ngx_http_v2_table.c"
	"src/http/v2/ngx_http_v2_encode.c"
	"src/http/v2/ngx_http_v2_module.c"
	"src/http/modules/ngx_http_static_module.c"
	"src/http/modules/ngx_http_dav_module.c"
	"src/http/modules/ngx_http_autoindex_module.c"
	"src/http/modules/ngx_http_index_module.c"
	"src/http/modules/ngx_http_mirror_module.c"
	"src/http/modules/ngx_http_try_files_module.c"
	"src/http/modules/ngx_http_auth_request_module.c"
	"src/http/modules/ngx_http_auth_basic_module.c"
	"src/http/modules/ngx_http_access_module.c"
	"src/http/modules/ngx_http_limit_conn_module.c"
	"src/http/modules/ngx_http_limit_req_module.c"
	"src/http/modules/ngx_http_realip_module.c"
	"src/http/modules/ngx_http_geo_module.c"
	"src/http/modules/ngx_http_map_module.c"
	"src/http/modules/ngx_http_split_clients_module.c"
	"src/http/modules/ngx_http_referer_module.c"
	"src/http/modules/ngx_http_rewrite_module.c"
	"src/http/modules/ngx_http_ssl_module.c"
	"src/http/modules/ngx_http_proxy_module.c"
	"src/http/modules/ngx_http_fastcgi_module.c"
	"src/http/modules/ngx_http_uwsgi_module.c"
	"src/http/modules/ngx_http_scgi_module.c"
	"src/http/modules/ngx_http_grpc_module.c"
	"src/http/modules/ngx_http_memcached_module.c"
	"src/http/modules/ngx_http_empty_gif_module.c"
	"src/http/modules/ngx_http_browser_module.c"
	"src/http/modules/ngx_http_flv_module.c"
	"src/http/modules/ngx_http_mp4_module.c"
	"src/http/modules/ngx_http_upstream_hash_module.c"
	"src/http/modules/ngx_http_upstream_ip_hash_module.c"
	"src/http/modules/ngx_http_upstream_least_conn_module.c"
	"src/http/modules/ngx_http_upstream_random_module.c"
	"src/http/modules/ngx_http_upstream_keepalive_module.c"
	"src/http/modules/ngx_http_upstream_zone_module.c"
	"src/mail/ngx_mail.c"
	"src/mail/ngx_mail_core_module.c"
	"src/mail/ngx_mail_handler.c"
	"src/mail/ngx_mail_parse.c"
	"src/mail/ngx_mail_ssl_module.c"
	"src/mail/ngx_mail_pop3_module.c"
	"src/mail/ngx_mail_pop3_handler.c"
	"src/mail/ngx_mail_imap_module.c"
	"src/mail/ngx_mail_imap_handler.c"
	"src/mail/ngx_mail_smtp_module.c"
	"src/mail/ngx_mail_smtp_handler.c"
	"src/mail/ngx_mail_auth_http_module.c"
	"src/mail/ngx_mail_proxy_module.c"
	"src/mail/ngx_mail_realip_module.c"
	"src/stream/ngx_stream.c"
	"src/stream/ngx_stream_variables.c"
	"src/stream/ngx_stream_script.c"
	"src/stream/ngx_stream_handler.c"
	"src/stream/ngx_stream_core_module.c"
	"src/stream/ngx_stream_log_module.c"
	"src/stream/ngx_stream_proxy_module.c"
	"src/stream/ngx_stream_upstream.c"
	"src/stream/ngx_stream_upstream_round_robin.c"
	"src/stream/ngx_stream_write_filter_module.c"
	"src/stream/ngx_stream_ssl_module.c"
	"src/stream/ngx_stream_realip_module.c"
	"src/stream/ngx_stream_limit_conn_module.c"
	"src/stream/ngx_stream_access_module.c"
	"src/stream/ngx_stream_geo_module.c"
	"src/stream/ngx_stream_map_module.c"
	"src/stream/ngx_stream_split_clients_module.c"
	"src/stream/ngx_stream_return_module.c"
	"src/stream/ngx_stream_set_module.c"
	"src/stream/ngx_stream_upstream_hash_module.c"
	"src/stream/ngx_stream_upstream_least_conn_module.c"
	"src/stream/ngx_stream_upstream_random_module.c"
	"src/stream/ngx_stream_upstream_zone_module.c"
	"objs/ngx_modules.c"
  )

add_executable(nginx ${SOURCE_FILES})

target_include_directories(nginx PRIVATE ${ALL_INCS})

target_link_options(nginx PRIVATE ${COMPI_OPT})



set(PREFIX "/usr/local/nginx")
set(PID_PATH "${PREFIX}/logs/nginx.pid")
set(CONF_PATH "${PREFIX}/conf/nginx.conf")
set(ERROR_LOG_PATH "${PREFIX}/logs/error.log")

# Custom command to generate objs/nginx.8 from man/nginx.8
# add_custom_command(
#     OUTPUT ${CMAKE_BINARY_DIR}/objs/nginx.8
#     COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_BINARY_DIR}/objs
#     COMMAND sed -e "s|%%PREFIX%%|${PREFIX}|" -e "s|%%PID_PATH%%|${PID_PATH}|" -e "s|%%CONF_PATH%%|${CONF_PATH}|" -e "s|%%ERROR_LOG_PATH%%|${ERROR_LOG_PATH}|" < ${CMAKE_SOURCE_DIR}/man/nginx.8 > ${CMAKE_BINARY_DIR}/objs/nginx.8
#     DEPENDS ${CMAKE_SOURCE_DIR}/man/nginx.8 ${CMAKE_SOURCE_DIR}/objs/ngx_auto_config.h
#     COMMENT "Generating nginx.8 man page"
# )
# 
# add_custom_target(manpage ALL DEPENDS ${CMAKE_BINARY_DIR}/objs/nginx.8)

install(TARGETS nginx DESTINATION ${PREFIX}/sbin)

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/koi-win  DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/koi-utf  DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/win-utf  DESTINATION ${PREFIX}/conf)

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/mime.types      DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/fastcgi_params  DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/fastcgi.conf    DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/uwsgi_params    DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/scgi_params     DESTINATION ${PREFIX}/conf)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/conf/nginx.conf      DESTINATION ${PREFIX}/conf)

install(DIRECTORY DESTINATION ${PREFIX}/logs)
install(DIRECTORY DESTINATION ${PREFIX}/html)

install(
    DIRECTORY ${CMAKE_SOURCE_DIR}/html
    DESTINATION ${PREFIX}/html
    FILES_MATCHING # 匹配文件而非目录
    PATTERN "*"    # 匹配所有文件
)
