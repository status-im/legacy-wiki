<span class="c38 c51 c55">Initialisation</span>

<span class="c36"></span>

<span>Whisper is initialised
</span><span class="c27">[here](https://www.google.com/url?q=https://github.com/status-im/status-react/blob/cede0957467739fb660f70944dd0fd2f2580e438/src/status_im/protocol/core.cljs%23L78&sa=D&ust=1510342037170000&usg=AFQjCNFERKsbHwORpanF4yoWjrWCKwjy-w)</span><span> in
our core.cljs file.</span>

<span class="c36"></span>

<span class="c14">1. Create filters for all groups. </span>

<span>In order to do this we first generate symmetric keys using the
group id as a password
(</span><span class="c28">shh.generateSymKeyFromPassword</span><span>).
Currently this key is used only as a “topic” for messages in group chat
(that’s how we distinguish group chats from each other). In whisper v2
we were allowed to use topics of any length, that’s why it was possible
to pass topic option to
</span><span class="c28">shh.newMessageFilter</span><span class="c36">.
But in whisper v5 topic can be only 4 bytes, which is not enough for our
purposes. </span>

<span>When we have symmetric key we can start listening to new messages
using
</span><span class="c28">shh.newMessageFilter</span><span class="c36"> with
next
parameters:</span>

<span class="c36"></span>

<span class="c5">{</span>

<span class="c29"> </span><span class="c29 c43">"sym"</span><span class="c29">:
</span><span class="c16">"generated sym
key"</span><span class="c5">,</span>

<span class="c29"> </span><span class="c43 c29">"topics"</span><span class="c29">:
\[</span><span class="c16">"hardcoded status topic
0xaabb11ee"</span><span class="c5">\]</span>

<span class="c5">}</span>

<span class="c12"></span>

<span class="c29 c51 c44">2. Create filter for 1-1
chats</span><span class="c12"> </span>

<span class="c29">The same
</span><span class="c28">shh.newMessageFilter</span><span class="c36"> is
used, parameters
are</span>

<span class="c36"></span>

<span class="c5">{</span>

<span class="c29"> </span><span class="c43 c29">"asym"</span><span class="c29">:
</span><span class="c16">"user's public
key"</span><span class="c5">,</span>

<span class="c29"> </span><span class="c43 c29">"topics"</span><span class="c29">:
\[</span><span class="c16">"0xaabb11ee"</span><span class="c5">\]</span>

<span class="c5">}</span>

<span class="c36"></span>

<span class="c36">Only one filter is created for all incoming 1-1
messages as we can distinguish senders by their signature. </span>

<span class="c36"></span>

<span class="c14">3. Create filters for profile updates</span>

<span>Unlike in group messages filters, we know who will send messages
and can use sig option here. Still symmetric key is used as messages are
broadcasted to all contacts. So
</span><span class="c28">shh.newMessageFilter</span><span class="c36"> parameters
are:</span>

<span class="c5">{</span>

<span class="c29"> </span><span class="c43 c29">"sig"</span><span class="c29">:
</span><span class="c16">"user's public
key"</span><span class="c5">,</span>

<span class="c29"> </span><span class="c43 c29">"sym"</span><span class="c29">:
</span><span class="c16">"key generated from hardcoded string
'status-discovery'"</span><span class="c5">,</span>

<span class="c29"> </span><span class="c43 c29">"topics"</span><span class="c29">:
\[</span><span class="c16">"0xaabb11ee"</span><span class="c5">\]</span>

<span class="c5">}</span>

<span class="c36"></span>

<span class="c51 c44">4. Create delivery loop
</span><span class="c36">(will be described in separate doc)</span>

<span class="c51 c44">5. Send
</span><span class="c31 c29 c44 c51">:online
</span><span class="c14">message</span>

<span class="c14"></span>

<span class="c14">Parameters</span>

<span class="c38 c10">{</span>

<span class="c17 c10">;; web3 object</span>

<span class="c10 c31">:web3                      
 </span><span class="c38 c10">web3</span>

<span class="c17 c10">;; user's public key</span>

<span class="c10 c31">:identity                  
 </span><span class="c38 c10">public-key</span>

<span class="c17 c10">;; vector of groups, where group is {:group-id
"id" :keypair {:private "" :public ""}}</span>

<span class="c10 c31">:groups                    
 </span><span class="c38 c10">groups</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; function which will be called on incoming
message,</span>

<span class="c17 c10">;; first parameter is message type, second
payload</span>

<span class="c10 c31">:callback                  
 </span><span class="c10">\#(</span><span class="c10 c54">dispatch
</span><span class="c10">\[</span><span class="c10 c31">:incoming-message
</span><span class="c38 c10">%1 %2\])</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; if message require ack and it wasn't received
during :ack-not-received-s-interval after previous </span>

<span class="c10 c17">;; attempt to send message message will be sent
again. Should be bigger than default-ttl.</span>

<span class="c17 c10">;; In seconds.</span>

<span class="c10 c31">:ack-not-received-s-interval
</span><span class="c33 c10">125</span>

<span class="c33 c10"></span>

<span class="c17 c10">;; default ttl for all messages in seconds</span>

<span class="c10 c31">:default-ttl                            
 </span><span class="c33 c10">120</span>

<span class="c10 c33"></span>

<span class="c17 c10">;; interval for sending :online message</span>

<span class="c10 c31">:send-online-s-interval        
</span><span class="c33 c10">180</span>

<span class="c33 c10"></span>

<span class="c17 c10">;; custom ttl config for specific message
types</span>

<span class="c10 c31">:ttl-config                
 </span><span class="c10">{ </span><span class="c17 c10">;; we don't
expect to receive acc for  :public-group-message, that's why ttl is
</span>

<span class="c17 c10">                                     ;;bigger
 than default. This increases chances for message</span>

<span class="c17 c10">                                     ;; delivery,
though guaranties nothing.</span>

<span class="c10 c26">                                   
 </span><span class="c10 c31">:public-group-message
</span><span class="c10 c46">2400</span><span class="c10 c38">}</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; Number of attempts of sending message if it
requires ack.</span>

<span class="c10 c31">:max-attempts-number        
</span><span class="c33 c10">3</span>

<span class="c33 c10"></span>

<span class="c17 c10">;; Interval for delivery loop. Details will be
described in separate doc.</span>

<span class="c10 c31">:delivery-loop-ms-interval  
</span><span class="c33 c10">500</span>

<span class="c33 c10"></span>

<span class="c17 c10">;; Keypair which is used for sending profile
updates and discovery</span>

<span class="c10 c31">:profile-keypair            
</span><span class="c10">{</span><span class="c10 c31">:public
 </span><span class="c38 c10">updates-public-key</span>

<span class="c10">                                       
</span><span class="c10 c31">:private
</span><span class="c38 c10">updates-private-key}</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; messages from db that require ack but haven't
received it</span>

<span class="c10 c31">:pending-messages          
 </span><span class="c10">(</span><span class="c10 c54">pending-messages/get-all</span><span class="c38 c10">)</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; all contacts which can send profile
updates/discovery</span>

<span class="c10 c31">:contacts                  
 </span><span class="c38 c10">      all-contacts</span>

<span class="c38 c10"></span>

<span class="c17 c10">;; callback for errors in \`shh.post\` call</span>

<span class="c10 c31">:post-error-callback        
</span><span class="c10">\#(</span><span class="c10 c54">dispatch
</span><span class="c10">\[</span><span class="c10 c31">::post-error
</span><span class="c38 c10">%\])}</span>

<span class="c14"></span>

<span class="c14">Delivery </span>

<span class="c12"></span>

1.  <span class="c29 c44">For each new message we create an entry of the
    </span><span class="c23">:pending-message</span><span class="c12"> entity
    and store it in db</span>
2.  <span class="c29 c44">Each new message is added to
    </span><span class="c23">status-im.protocol.web3.delivery/messages</span><span class="c12"> map</span>
3.  <span class="c29 c44">All
    </span><span class="c23">:pending-message</span><span class="c29 c44"> entries
    from db are added to
    </span><span class="c23">status-im.protocol.web3.delivery/messages</span><span class="c12"> on
    whisper initialisation</span>
4.  <span class="c29 c44">All messages from
    </span><span class="c23">status-im.protocol.web3.delivery/messages</span><span class="c12"> are
    sent over Whisper inside delivery loop</span>
5.  <span class="c29 c44">Messages which don’t require ack are removed
    from both
    </span><span class="c23">:pending-message</span><span class="c29 c44"> and
    </span><span class="c23">status-im.protocol.web3.delivery/messages</span><span class="c12"> after
    successful call to shh.post</span>
6.  <span class="c29 c44">Messages that require ack are removed from
    </span><span class="c23">pending-message</span><span class="c29 c44"> and
    </span><span class="c23">status-im.protocol.web3.delivery/messages</span><span class="c12"> only
    after receiving ack.</span>

<span class="c14"></span>

<span class="c14">Delivery loop</span>

<span class="c14"></span>

<span class="c44">The delivery loop is started
</span><span class="c27 c44">[here](https://www.google.com/url?q=https://github.com/status-im/status-react/blob/43a5d91ca2702cfb63cc57c2eac2c5193d77aa8f/src/status_im/protocol/web3/delivery.cljs%23L204&sa=D&ust=1510342037188000&usg=AFQjCNF6M0QH3VXk43UANmSIZYJhS4DUDw)</span><span class="c29 c44"> in
our delivery.cljs file.</span>

<span class="c12"></span>

<span class="c12">Parameters:</span>

<span class="c12"></span>

<span class="c29">{</span><span class="c17 c52 c29">;; function which
sends :online
message</span>

<span class="c17 c52 c29">;;https://github.com/status-im/status-react/blob/cede0957467739fb660f70944dd0fd2f2580e438/src/status_im/protocol/core.cljs\#L112</span>

<span class="c31 c29">:online-message  
 </span><span class="c5">fn</span>

<span class="c5"></span>

<span class="c17 c29 c52">;; other parameter are same as for whisper
initialisation</span>

<span class="c31 c29">:delivery-loop-ms-interval
</span><span class="c5">...</span>

<span class="c31 c29">:ack-not-received-s-interval
 </span><span class="c5">...</span>

<span class="c31 c29">:max-attempts-number
</span><span class="c5">...</span>

<span class="c31 c29">:default-ttl </span><span class="c5">...</span>

<span class="c31 c29">:send-online-s-interval
</span><span class="c5">...}</span>

<span class="c5"></span>

<span class="c29 c44">Delivery loop is running with
</span><span class="c23">delivery-loop-ms-interval</span><span class="c29 c44"> interval,
it is 500ms</span><span class="c29 c44 c46"> </span><span class="c12">by
default.</span>

<span class="c12"></span>

<span class="c29 c44">In order to check if message should be sent in the
current iteration of delivery loop we check message with
</span><span class="c23 c27">[status-im.protocol.web3.delivery/should-be-retransmitted?](https://www.google.com/url?q=https://github.com/status-im/status-react/blob/develop/src/status_im/protocol/web3/delivery.cljs&sa=D&ust=1510342037191000&usg=AFQjCNHeBrMmgh974VJ5Jgeaxh5AA6mSRw)</span><span class="c12"> function,
message will be sent if:</span>

1.  <span class="c29 c44">Wasn’t previously successfully sent via
    </span><span class="c23">shh.posts</span><span class="c29 c44">(during
    current session), we retry to do this
    </span><span class="c31 c23">max-attempts-number \*
    5</span><span class="c12"> times</span>
2.  <span class="c12">Was successfully sent, but requires ack which
    wasn’t received </span>

<span class="c29 c44">+ previous attempt to send message (during current
session) was done more than
</span><span class="c31 c23">ack-not-received-s-interval</span><span class="c31 c29 c51 c44"> </span><span class="c12">seconds
ago</span>

<span class="c29 c44">+ we did not more than
</span><span class="c31 c23">max-attempts-number</span><span class="c31 c29 c44"> </span><span class="c12">attempts
previously (during current session)</span>

<span class="c12"></span>

<span class="c14 c29">Keys</span>

<span class="c12"></span>

<span class="c39">Key type
</span><span class="c32">asym</span><span class="c39"> (asymmetric)
means that asymmetric key will be used on whisper side to encrypt
message before sending, in our case it's always user's public key, this
means that currently messages with
</span><span class="c32">asym</span><span class="c2"> key type are
supposed to be sent from UserA to UserB directly, but not to any group
of users.</span>

<span class="c39">Key type
</span><span class="c32">sym</span><span class="c2"> (symmetric) means
that symmetric key will be used to encrypt message. Currently all
symkeys are generated from passwords which are hardcoded or are groups'
ids, so this is not secure and must be
changed.</span>

<span id="t.3b752c6106f49a6f20d0ad7b32e930254e9cc384"></span><span id="t.0"></span>

<table style="width:99%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><span class="c39 c51">Type</span></p></td>
<td><p><span class="c39 c51">Description</span></p></td>
<td><p><span class="c39 c51">Key type</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23message&amp;sa=D&amp;ust=1510342037195000&amp;usg=AFQjCNGZL98xHJDnANhi4adkbY3PXYOUsQ">:message</a></span></p></td>
<td><p><span class="c2">1-1 chat message</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23group-message&amp;sa=D&amp;ust=1510342037196000&amp;usg=AFQjCNFdl6brk2HvuJUIL0XDDBfp4i6InQ">:group-message</a></span></p></td>
<td><p><span class="c2">group message</span></p></td>
<td><p><span class="c2">sym (group's id)</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23public-group-message&amp;sa=D&amp;ust=1510342037198000&amp;usg=AFQjCNHeunbmcp0DDtkfczCgKsZmoUzrEw">:public-group-message</a></span></p></td>
<td><p><span class="c2">public group message</span></p></td>
<td><p><span class="c2">sym (group's id)</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23ack&amp;sa=D&amp;ust=1510342037199000&amp;usg=AFQjCNESp0oPHLzZ82FmGgtBoghigKwnIg">:ack</a></span></p></td>
<td><p><span class="c2">confirms message delivery</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23seen&amp;sa=D&amp;ust=1510342037201000&amp;usg=AFQjCNEj8nKO4DhPNDaYUFJZHVKFVKLmbA">:seen</a></span></p></td>
<td><p><span class="c2">confirms that message was seen</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23clock-value-request&amp;sa=D&amp;ust=1510342037202000&amp;usg=AFQjCNG0V0Vqr0PiqsM9nCIMdECfCqfeuw">:clock-value-request</a></span></p></td>
<td><p><span class="c2">when you send a message and you’re offline,</span></p>
<p><span class="c39">we send </span><span class="c32">:clock-value-request</span><span class="c2"> to obtain the information </span></p>
<p><span class="c39">about the </span><span class="c32">:clock-value</span><span class="c2"> from sender </span></p>
<p><span class="c2">to make sure that it’s the same</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23clock-value&amp;sa=D&amp;ust=1510342037204000&amp;usg=AFQjCNHixpfMhhCAyubOTwTPqGPjeNc2Sw">:clock-value</a></span></p></td>
<td><p><span class="c2">?</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23group-invitation&amp;sa=D&amp;ust=1510342037205000&amp;usg=AFQjCNEdxAbBXNXGqnvTP2zcHaaAXdeGeQ">:group-invitation</a></span></p></td>
<td><p><span class="c2">is sent by admin</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23update-group&amp;sa=D&amp;ust=1510342037207000&amp;usg=AFQjCNFWgHAYh0UVOL0_tbhYj71987tkaw">:update-group</a></span></p></td>
<td><p><span class="c2">currently this message is sent when some user</span></p>
<p><span class="c2">is removed from group, in result group's keypair is updated</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23add-group-identity&amp;sa=D&amp;ust=1510342037209000&amp;usg=AFQjCNFq22SACWNL-Mm8FpDjFQKnonnnRw">:add-group-identity</a></span></p></td>
<td><p><span class="c2">is sent when a new user is added to group</span></p></td>
<td><p><span class="c2">sym (group's id)</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23remove-group-identity&amp;sa=D&amp;ust=1510342037211000&amp;usg=AFQjCNHCVzy35fKMvV1abtc-QKDP1-EuLA">:remove-group-identity</a></span></p></td>
<td><p><span class="c2">is sent when some user is removed from group (by admin),</span></p>
<p><span class="c2">after that :update-group with new keypair is sent</span></p></td>
<td><p><span class="c2">sym (group's id)</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23leave-group&amp;sa=D&amp;ust=1510342037213000&amp;usg=AFQjCNG5UY9lJ6533DOr8SfVs7_Ij-XsMg">:leave-group</a></span></p></td>
<td><p><span class="c2">is sent when user leaves group</span></p></td>
<td><p><span class="c2">sym (group's id)</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23contact-request&amp;sa=D&amp;ust=1510342037215000&amp;usg=AFQjCNFMME9FMkc5pnRLkKa-R0ra2e2FGA">:contact-request</a></span></p></td>
<td><p><span class="c2">-</span></p></td>
<td><p><span class="c2">asym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23discover&amp;sa=D&amp;ust=1510342037217000&amp;usg=AFQjCNGbPdvpBJnftebm6liVnXwM9s7Xfg">:discover</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23discoveries-request&amp;sa=D&amp;ust=1510342037218000&amp;usg=AFQjCNEW8NdZ5n2yWoVFZdieWqVhefqq1g">:discoveries-request</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23discoveries-response&amp;sa=D&amp;ust=1510342037220000&amp;usg=AFQjCNFGrhcXAXHe3wVMeBcKeyLoai8Z_Q">:discoveries-response</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23profile&amp;sa=D&amp;ust=1510342037222000&amp;usg=AFQjCNE-rF3n_1EX_7oX8V-MDOBCB8vszw">:profile</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
<tr class="odd">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23update-keys&amp;sa=D&amp;ust=1510342037223000&amp;usg=AFQjCNG4zaiDHfoPH7jN-NDBmPSBnxztPQ">:update-keys</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
<tr class="even">
<td><p><span class="c34"><a href="https://www.google.com/url?q=https://gist.github.com/rasom/877378fe34e113ef762d526329f25111%23online&amp;sa=D&amp;ust=1510342037225000&amp;usg=AFQjCNF6qZl-EmxfRddEO8NdagnwY3ipSg">:online</a></span></p></td>
<td><p><span class="c2"></span></p></td>
<td><p><span class="c2">sym</span></p></td>
</tr>
</tbody>
</table>

## <span class="c37">:message</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498723691404-7711c1b3-411c-5e4b-a64b-dc0ab2317c5a"</span><span class="c22">,
</span><span class="c3">:requires-ack?</span><span class="c22"> </span><span class="c3">true</span><span class="c22">,
</span><span class="c3">:type</span><span class="c22">         
</span><span class="c3">:message</span><span class="c22">,
</span><span class="c3">:timestamp</span><span class="c22">   
 </span><span class="c3">1498723691404</span><span class="c22">,
</span><span class="c3">:content</span><span class="c22">     
 </span><span class="c24">"123"</span><span class="c22">,
</span><span class="c3">:content-type</span><span class="c22"> 
</span><span class="c24">"text/plain"</span><span class="c22">,
</span><span class="c3">:clock-value</span><span class="c22"> 
 </span><span class="c3">1</span><span class="c22">,
</span><span class="c3">:show?</span><span class="c22">       
 </span><span class="c3">true</span><span class="c22 c45">}</span>

## <span class="c37">:group-message</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498724252836-24753a20-b67e-50ae-8fcd-95b22a1ee78b"</span><span class="c22">,
</span><span class="c3">:group-id</span><span class="c22">     
</span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
</span><span class="c3">:content</span><span class="c22">     
 </span><span class="c24">"{\\"iv\\":\[2071556246,-1120403611,-1962404966,921696242\],\\"v\\":1,\\"iter\\":1000,\\"ks\\":128,\\"ts\\":64,\\"mode\\":\\"ccm\\",\\"adata\\":\\"\\",\\"cipher\\":\\"aes\\",\\"ct\\":\[-1840979203,1955718739,-1431291835,8794767622144\],\\"tag\\":\\"75b9b052a2c2835d8ae9525bd6ae3245e879f4b4b8b510fca22b4ad142a2f10913a83c6ff87e80ef766e194d5017cd1eca6c27fb41611cf9c8d8cf3a5df8980b6959d6d66bbfc9e4cf8a6af27decc1663f60a9e201992236a9ea21b2ccab7284\\"}"</span><span class="c22">,
</span><span class="c3">:username</span><span class="c22">      nil,
</span><span class="c3">:type</span><span class="c22">         
</span><span class="c3">:group-message</span><span class="c22">,
</span><span class="c3">:show?</span><span class="c22">       
 </span><span class="c3">true</span><span class="c22">,
</span><span class="c3">:clock-value</span><span class="c22"> 
 </span><span class="c3">1</span><span class="c22">,
</span><span class="c3">:requires-ack?</span><span class="c22"> </span><span class="c3">true</span><span class="c22">,
</span><span class="c3">:content-type</span><span class="c22"> 
</span><span class="c24">"text/plain"</span><span class="c22">,
</span><span class="c3">:timestamp</span><span class="c22">   
 </span><span class="c3">1498724252865</span><span class="c22 c45">}
</span>

## <span class="c37">:public-group-message</span>

<span class="c20">{</span><span class="c35">:message-id</span><span class="c20"> 
 
</span><span class="c41">"1498724827175-9c097aa0-d859-5ddd-bdd1-0c83e52d930e"</span><span class="c20">,
</span><span class="c35">:group-id</span><span class="c20">     
</span><span class="c41">"wow"</span><span class="c20">,
</span><span class="c35">:content</span><span class="c20">     
 </span><span class="c41">"123"</span><span class="c20">,
</span><span class="c35">:username</span><span class="c20">     
</span><span class="c41">"Name"</span><span class="c20">,
</span><span class="c35">:type</span><span class="c20">         
</span><span class="c35">:public-group-message</span><span class="c20">,
</span><span class="c35">:show?</span><span class="c20">       
 </span><span class="c35">true</span><span class="c20">,
</span><span class="c35">:clock-value</span><span class="c20"> 
 </span><span class="c35">1</span><span class="c20">,
</span><span class="c35">:requires-ack?</span><span class="c20"> </span><span class="c35">false</span><span class="c20">,
</span><span class="c35">:content-type</span><span class="c20"> 
</span><span class="c41">"text/plain"</span><span class="c20">,
</span><span class="c35">:timestamp</span><span class="c20">   
 </span><span class="c35">1498724827202</span><span class="c20 c45">}</span>

## <span class="c37">:contact-request</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498723171063-c3ecb16a-b3fd-5ee1-99f0-783ef7a1070c"</span><span class="c22">,
</span><span class="c3">:requires-ack?</span><span class="c22"> </span><span class="c3">true</span><span class="c22">,
</span><span class="c3">:type</span><span class="c22">         
</span><span class="c3">:contact-request</span><span class="c22">,
</span><span class="c3">:contact</span><span class="c22">     
 {</span><span class="c3">:name</span><span class="c22">         
</span><span class="c24">"Yearly Onerlooked
Westafricanantelope"</span><span class="c22">,
               
</span><span class="c3">:profile-image</span><span class="c22"> </span><span class="c24">"..."</span><span class="c22">},
</span><span class="c18">;; public key is used by UserA (who sends this
request) for broadcasting changes of his profile to UserB
</span><span class="c22">
</span><span class="c18">;; (and other contacts)
</span><span class="c22">
</span><span class="c3">:keypair</span><span class="c22">     
 {</span><span class="c3">:public</span><span class="c22"> 
</span><span class="c24">".."</span><span class="c22">,
               
</span><span class="c3">:private</span><span class="c22"> </span><span class="c24">".."</span><span class="c22">},
</span><span class="c3">:content</span><span class="c22">     
 </span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:online</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498723263025-a83a4fe0-eff6-518d-9348-2570d1494067"</span><span class="c22">,
</span><span class="c3">:requires-ack?</span><span class="c22"> </span><span class="c3">false</span><span class="c22">,
</span><span class="c3">:type</span><span class="c22">         
</span><span class="c3">:online</span><span class="c22">,
</span><span class="c3">:content</span><span class="c22">     
 </span><span class="c24">"{\\"iv\\":\[1966114636,-1876596685,-393541684,-299376444\],\\"v\\":1,\\"iter\\":1000,\\"ks\\":128,\\"ts\\":64,\\"mode\\":\\"ccm\\",\\"adata\\":\\"\\",\\"cipher\\":\\"aes\\",\\"ct\\":\[-1468463324,357257435,1338099857,1642960690,-1068289456,705936716,-1541701378,-565102533,26389113252352\],\\"tag\\":\\"3151bd11093fdeb90ccdf311b660386b2edb959a5c52d9ed63095fb9ac9bf3d9bdb7ce3841dce1d2820c04d43ca2548c171c4049e06e498b35e6bea3e210afd8dfb32437663e1497a8d3ed7772b72160edc69f124e69e2385ca19cfc5692c7f9\\"}"</span><span class="c22 c45">}</span>

## <span class="c37">:profile</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> </span><span class="c24">"1498723799459-6642807d-2b32-58cd-98d6-19359ad3d8da"</span><span class="c22">,
</span><span class="c3">:type</span><span class="c22">     
 </span><span class="c3">:profile</span><span class="c22">,
</span><span class="c3">:timestamp</span><span class="c22"> 
</span><span class="c3">1498723799465</span><span class="c22">,
</span><span class="c3">:content</span><span class="c22">   
</span><span class="c24">"encrypted content: username, status, photo
etc"</span><span class="c22 c45">}
</span>

## <span class="c37">:group-invitation</span>

<span class="c22">{</span><span class="c3">:group-admin</span><span class="c22"> 
 </span><span class="c24">"0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"</span><span class="c22">,
:message-id  
 </span><span class="c24">"1498723975437-09f7ca06-2ffe-5067-87d0-a65d128efb31"</span><span class="c22">,
:contacts    
 \[</span><span class="c24">"0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b"</span><span class="c22">
             
 </span><span class="c24">"0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"</span><span class="c22">\],
:group-id    
 </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:content       nil,
:type        
 </span><span class="c3">:group-invitation</span><span class="c22">,
:requires-ack? </span><span class="c3">true</span><span class="c22">,
:group-name    </span><span class="c24">"T"</span><span class="c22">,
</span><span class="c18">;; this keypair is used for sending messages to
all group members, so that any user could encrypt/decrypt
message</span><span class="c22">
:keypair      
{</span><span class="c3">:public</span><span class="c22"> 
</span><span class="c24">".."</span><span class="c22">,
             
 </span><span class="c3">:private</span><span class="c22"> </span><span class="c24">".."</span><span class="c22">},
:timestamp    
</span><span class="c3">1498723975439</span><span class="c22">}</span><span class="c20 c45">
</span>

## <span class="c37"></span>

## <span class="c37"></span>

## <span class="c37">:discoveries-request</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> </span><span class="c24">"1498724283011-65da6bd7-1cb0-566f-a055-27d66cbd04e4"</span><span class="c22">,
:type      
</span><span class="c3">:discoveries-request</span><span class="c22">,
:content  
 </span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:remove-group-dentity</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498724483015-0d468376-69f5-551c-a6c1-3762f7cb9651"</span><span class="c22">,
:requires-ack? </span><span class="c3">true</span><span class="c22">,
:type        
 </span><span class="c3">:remove-group-identity</span><span class="c22">,
:identity      </span><span class="c24">"id"</span><span class="c22">,
:group-id    
 </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:username      nil,
:timestamp    
</span><span class="c3">1498724483015</span><span class="c22">,
:content      
</span><span class="c3">nil</span><span class="c22 c45">}</span>

<span class="c37">:update-group</span>

<span class="c22">{</span><span class="c3">:group-admin</span><span class="c22"> 
 </span><span class="c24">"0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"</span><span class="c22">,
:message-id  
 </span><span class="c24">"1498725030208-c44f66bb-4d9c-5022-b992-eb663511792a"</span><span class="c22">,
:contacts    
 (</span><span class="c19">"0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"</span><span class="c22"> 
           
</span><span class="c24">"0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b"</span><span class="c22">),
:group-id    
 </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:content       nil,
:type        
 </span><span class="c3">:update-group</span><span class="c22">,
:requires-ack? </span><span class="c3">true</span><span class="c22">,
:group-name  
 </span><span class="c24">"T"</span><span class="c22 c45">,</span>

<span class="c22">:keypair      
{</span><span class="c3">:private</span><span class="c22"> </span><span class="c24">".."</span><span class="c22">,
</span><span class="c3">:public</span><span class="c22"> 
</span><span class="c24">".."</span><span class="c22">},
:timestamp    
</span><span class="c3">1498725030220</span><span class="c22 c45">}</span>

## <span class="c37">:add-group-identity</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498725030221-3a4f2890-3897-546b-8819-c240d99089a3"</span><span class="c22">,
:requires-ack? </span><span class="c3">true</span><span class="c22">,
:type        
 </span><span class="c3">:add-group-identity</span><span class="c22">,
:identity    
 </span><span class="c24">"0x0428c9d6c1aaaa8369a7c63819684f30e34396dc0907d49afeac85a0a774ccb919b3482097d992e66bcc538e7a0c6acf874c77748f396f53c0a102e10d1a37765b"</span><span class="c22">,
</span><span class="c3">:group-id</span><span class="c22"> </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:username      nil,
:timestamp    
</span><span class="c3">1498725030222</span><span class="c22">,
:content      
</span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:seen</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498724483032-ef9f6889-5699-5603-9ffd-05bee945f146"</span><span class="c22">,
:requires-ack? </span><span class="c3">false</span><span class="c22">,
:type          </span><span class="c3">:seen</span><span class="c22">,
:group-id    
 </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:content       </span><span class="c3">nil</span><span class="c22">}
{</span><span class="c3">:message-id</span><span class="c22">   
</span><span class="c24">"1498725866330-4a77d7d9-5d18-5628-8cf4-1f7d862f5122"</span><span class="c22">,
:requires-ack? </span><span class="c3">false</span><span class="c22">,
:type          </span><span class="c3">:seen</span><span class="c22">,
:group-id      nil,
:content      
</span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:leave-group</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
</span><span class="c24">"1498725404895-36369ea8-1403-5554-b921-c58aaa8d1e0a"</span><span class="c22">,
:requires-ack? </span><span class="c3">true</span><span class="c22">,
:type        
 </span><span class="c3">:leave-group</span><span class="c22">,
:group-id    
 </span><span class="c24">"1498723975430-5a3f18f5-189e-5966-b159-bdf837d47e58"</span><span class="c22">,
:username      nil,
:timestamp    
</span><span class="c3">1498725404895</span><span class="c22">,
:content      
</span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:discover</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> </span><span class="c24">"1498725562980-10a58c17-b994-5dd3-87b1-cbdc30533df0"</span><span class="c22">,
:type      
</span><span class="c3">:discover</span><span class="c22">,
:status     </span><span class="c24">"the \#life we're given is on a
thread, so wear it well"</span><span class="c22">,
:hashtags  
\[</span><span class="c24">"life"</span><span class="c22">\],
:profile    {</span><span class="c3">:name</span><span class="c22">     
    </span><span class="c24">"Name"</span><span class="c22">,
           
</span><span class="c3">:profile-image</span><span class="c22"> </span><span class="c24">"<data:image/png;base64>,..."</span><span class="c22">},
:content  
 </span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:discoveries-response</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> </span><span class="c24">"1498725718717-2c483805-da99-568b-8958-1e9a54d30630"</span><span class="c22">,
:type      
</span><span class="c3">:discoveries-response</span><span class="c22">,
:data      
\[{</span><span class="c3">:message-id</span><span class="c22"> </span><span class="c24">"1498725562980-10a58c17-b994-5dd3-87b1-cbdc30533df"</span><span class="c22">,
             </span><span class="c3">:name</span><span class="c22">     
 </span><span class="c24">"Name"</span><span class="c22">,
             </span><span class="c3">:status</span><span class="c22">   
 </span><span class="c24">"the \#life we're given is on a
thread"</span><span class="c22">,
           
 </span><span class="c3">:whisper-id</span><span class="c22"> </span><span class="c24">"0x04e40d8b966a6644bb8ef2a5c1e170c7c0ef9ba83e8d0cb2e2f07930d092af51cdec3aab1c97b1c451460d0e3525fd5b079ab32ba1ff179fc0db2f047ddb71a703"</span><span class="c22">,
           
 </span><span class="c3">:photo-path</span><span class="c22"> </span><span class="c24">"<data:image/png;base64>,..."</span><span class="c22">,
             </span><span class="c3">:tags</span><span class="c22">     
 ({</span><span class="c3">:name</span><span class="c22"> 
</span><span class="c24">"life"</span><span class="c22">,
                         
 </span><span class="c3">:count</span><span class="c22"> </span><span class="c3">1</span><span class="c22">}),
           
 </span><span class="c3">:created-at</span><span class="c22"> </span><span class="c3">1498725562990</span><span class="c22">}\],
:content  
 </span><span class="c3">nil</span><span class="c22 c45">}</span>

## <span class="c37">:ack</span>

<span class="c22">{</span><span class="c3">:message-id</span><span class="c22"> 
 
 </span><span class="c24">"1498725867070-78e60ec0-266e-5dba-a9f6-e53a333f38c7"</span><span class="c22">,
:type          
</span><span class="c3">:message</span><span class="c22">,
:ack?           </span><span class="c3">true</span><span class="c22">,
:ack-of-message
</span><span class="c24">"1498725866330-4a77d7d9-5d18-5628-8cf4-1f7d862f5122"</span><span class="c22">,
:group-id       nil,
:content      
 </span><span class="c3">nil</span><span class="c22 c45">}</span>

<span class="c22"><https://gist.github.com/rasom/877378fe34e113ef762d526329f25111></span>

<span class="c36"></span>