> 场景：在本地搭建jekyll框架，使用ruby编写的webrick作为服务器的时候，会出现浏览器正常访问http://localdomain:4000/404.html(中文不乱码),但当访问http://localdomain:4000/fuck(任意不存在的页面)，应当跳转到404.html,但此时中文却显示乱码  

### 解决方式
> 该问题是因为后端服务器返回的content-type为text/html;charset=ISO-8859-1  

> vi /usr/local/lib/ruby/2.5.0/webrick/httpresponse.rb  

  ```ruby
  #修改第351行为
  @header['content-type'] = "text/html; charset=UTF-8"
  ```
方法解释：当响应头设置失败的时候，会调用set_error方法，具体为什么设置头会失败,想深究的人可以阅读下源代码，此处不做深究
```ruby
# Creates an error page for exception +ex+ with an optional +backtrace+
# Sends the headers on +socket+

def send_header(socket) # :nodoc:
 if @http_version.major > 0
   data = status_line()
   @header.each{|key, value|
     tmp = key.gsub(/\bwww|^te$|\b\w/){ $&.upcase }
     data << "#{tmp}: #{check_header(value)}" << CRLF
   }
   @cookies.each{|cookie|
     data << "Set-Cookie: " << check_header(cookie.to_s) << CRLF
   }
   data << CRLF
   socket.write(data)
 end
rescue InvalidHeader => e
 @header.clear
 @cookies.clear
 set_error e
 retry
end
#
def set_error(ex, backtrace=false)
  case ex
  when HTTPStatus::Status
    @keep_alive = false if HTTPStatus::error?(ex.code)
    self.status = ex.code
  else
    @keep_alive = false
    self.status = HTTPStatus::RC_INTERNAL_SERVER_ERROR
  end
  @header['content-type'] = "text/html; charset=UTF-8"

  if respond_to?(:create_error_page)
    create_error_page()
    return
  end

  if @request_uri
    host, port = @request_uri.host, @request_uri.port
  else
    host, port = @config[:ServerName], @config[:Port]
  end

   error_body(backtrace, ex, host, port)
  end
```
