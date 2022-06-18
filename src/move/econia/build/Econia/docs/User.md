
<a name="0xc0deb00c_User"></a>

# Module `0xc0deb00c::User`

User-side functionality. Like <code>Econia::Registry</code>, relies on
<code>AptosFramework</code> functions and an associated pure-Move
module (<code>Econia::Orders</code>) for coverage testing.


-  [Resource `OC`](#0xc0deb00c_User_OC)
-  [Resource `OICC`](#0xc0deb00c_User_OICC)
-  [Constants](#@Constants_0)
-  [Function `init_o_i_c_c`](#0xc0deb00c_User_init_o_i_c_c)
-  [Function `init_user`](#0xc0deb00c_User_init_user)
-  [Function `exists_o_c`](#0xc0deb00c_User_exists_o_c)
-  [Function `init_o_c`](#0xc0deb00c_User_init_o_c)


<pre><code><b>use</b> <a href="../../../build/AptosFramework/docs/Coin.md#0x1_Coin">0x1::Coin</a>;
<b>use</b> <a href="../../../build/MoveStdlib/docs/Signer.md#0x1_Signer">0x1::Signer</a>;
<b>use</b> <a href="Orders.md#0xc0deb00c_Orders">0xc0deb00c::Orders</a>;
<b>use</b> <a href="Registry.md#0xc0deb00c_Registry">0xc0deb00c::Registry</a>;
</code></pre>



<a name="0xc0deb00c_User_OC"></a>

## Resource `OC`

Order collateral for a given market


<pre><code><b>struct</b> <a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt; <b>has</b> key
</code></pre>



<details>
<summary>Fields</summary>


<dl>
<dt>
<code>b_a: u64</code>
</dt>
<dd>
 Indivisible subunits of base coins available to withdraw
</dd>
<dt>
<code>b_c: <a href="../../../build/AptosFramework/docs/Coin.md#0x1_Coin_Coin">Coin::Coin</a>&lt;B&gt;</code>
</dt>
<dd>
 Base coins held as collateral
</dd>
<dt>
<code>q_a: u64</code>
</dt>
<dd>
 Indivisible subunits of quote coins available to withdraw
</dd>
<dt>
<code>q_c: <a href="../../../build/AptosFramework/docs/Coin.md#0x1_Coin_Coin">Coin::Coin</a>&lt;Q&gt;</code>
</dt>
<dd>
 Quote coins held as collateral
</dd>
</dl>


</details>

<a name="0xc0deb00c_User_OICC"></a>

## Resource `OICC`

Open orders initialization capability container


<pre><code><b>struct</b> <a href="User.md#0xc0deb00c_User_OICC">OICC</a> <b>has</b> key
</code></pre>



<details>
<summary>Fields</summary>


<dl>
<dt>
<code>o_i_c: <a href="Orders.md#0xc0deb00c_Orders_OrdersInitCap">Orders::OrdersInitCap</a></code>
</dt>
<dd>

</dd>
</dl>


</details>

<a name="@Constants_0"></a>

## Constants


<a name="0xc0deb00c_User_E_NOT_ECONIA"></a>

When account/address is not Econia


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_NOT_ECONIA">E_NOT_ECONIA</a>: u64 = 2;
</code></pre>



<a name="0xc0deb00c_User_E_HAS_OICC"></a>

When open orders initialization capability container already
published


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_HAS_OICC">E_HAS_OICC</a>: u64 = 3;
</code></pre>



<a name="0xc0deb00c_User_E_NO_MARKET"></a>

When no corresponding market


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_NO_MARKET">E_NO_MARKET</a>: u64 = 1;
</code></pre>



<a name="0xc0deb00c_User_E_NO_OICC"></a>

When Econia does not have open orders initialization capability


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_NO_OICC">E_NO_OICC</a>: u64 = 5;
</code></pre>



<a name="0xc0deb00c_User_E_O_C_EXISTS"></a>

When order collateral container already exists


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_O_C_EXISTS">E_O_C_EXISTS</a>: u64 = 0;
</code></pre>



<a name="0xc0deb00c_User_E_O_O_EXISTS"></a>

When open orders container already exists


<pre><code><b>const</b> <a href="User.md#0xc0deb00c_User_E_O_O_EXISTS">E_O_O_EXISTS</a>: u64 = 4;
</code></pre>



<a name="0xc0deb00c_User_init_o_i_c_c"></a>

## Function `init_o_i_c_c`

Publish <code><a href="User.md#0xc0deb00c_User_OICC">OICC</a></code> to Econia acount, aborting for all other accounts


<pre><code><b>public</b>(<b>script</b>) <b>fun</b> <a href="User.md#0xc0deb00c_User_init_o_i_c_c">init_o_i_c_c</a>(account: &signer)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>public</b>(<b>script</b>) <b>fun</b> <a href="User.md#0xc0deb00c_User_init_o_i_c_c">init_o_i_c_c</a>(
    account: &signer
) {
    // Assert account is Econia
    <b>assert</b>!(s_a_o(account) == @Econia, <a href="User.md#0xc0deb00c_User_E_NOT_ECONIA">E_NOT_ECONIA</a>);
    // Assert capability container not already initialized
    <b>assert</b>!(!<b>exists</b>&lt;<a href="User.md#0xc0deb00c_User_OICC">OICC</a>&gt;(@Econia), <a href="User.md#0xc0deb00c_User_E_HAS_OICC">E_HAS_OICC</a>);
    // Move orders initialization capability container <b>to</b> account
    <b>move_to</b>(account, <a href="User.md#0xc0deb00c_User_OICC">OICC</a>{o_i_c: o_g_o_i_c(account)});
}
</code></pre>



</details>

<a name="0xc0deb00c_User_init_user"></a>

## Function `init_user`

Initialize a user with <code>Econia::Orders::OO</code> and <code><a href="User.md#0xc0deb00c_User_OC">OC</a></code> for market
with base coin type <code>B</code>, quote coin type <code>Q</code>, and scale exponent
<code>E</code>, aborting if no such market or if user already initialized
for market


<pre><code><b>public</b>(<b>script</b>) <b>fun</b> <a href="User.md#0xc0deb00c_User_init_user">init_user</a>&lt;B, Q, E&gt;(user: &signer)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>public</b>(<b>script</b>) <b>fun</b> <a href="User.md#0xc0deb00c_User_init_user">init_user</a>&lt;B, Q, E&gt;(
    user: &signer
) <b>acquires</b> <a href="User.md#0xc0deb00c_User_OICC">OICC</a> {
    <b>assert</b>!(r_i_r&lt;B, Q, E&gt;(), <a href="User.md#0xc0deb00c_User_E_NO_MARKET">E_NO_MARKET</a>); // Assert market <b>exists</b>
    <b>let</b> user_addr = s_a_o(user); // Get user <b>address</b>
    // Assert user does not already have collateral container
    <b>assert</b>!(!<b>exists</b>&lt;<a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;&gt;(user_addr), <a href="User.md#0xc0deb00c_User_E_O_C_EXISTS">E_O_C_EXISTS</a>);
    // Assert user does not already have open orders
    <b>assert</b>!(!o_e_o&lt;B, Q, E&gt;(user_addr), <a href="User.md#0xc0deb00c_User_E_O_O_EXISTS">E_O_O_EXISTS</a>);
    // Assert Econia account <b>has</b> orders initialization capability
    <b>assert</b>!(<b>exists</b>&lt;<a href="User.md#0xc0deb00c_User_OICC">OICC</a>&gt;(@Econia), <a href="User.md#0xc0deb00c_User_E_NO_OICC">E_NO_OICC</a>);
    // Pack empty collateral container
    <b>let</b> o_c = <a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;{b_c: c_z&lt;B&gt;(), b_a: 0, q_c: c_z&lt;Q&gt;(), q_a: 0};
    <b>move_to</b>&lt;<a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;&gt;(user, o_c); // Move <b>to</b> user account
    // Borrow immutable reference <b>to</b> open orders init capability
    <b>let</b> o_i_c = &<b>borrow_global</b>&lt;<a href="User.md#0xc0deb00c_User_OICC">OICC</a>&gt;(@Econia).o_i_c;
    // Initialize empty open orders container under user account
    o_i_o&lt;B, Q, E&gt;(user, r_s_f&lt;E&gt;(), o_i_c);
}
</code></pre>



</details>

<a name="0xc0deb00c_User_exists_o_c"></a>

## Function `exists_o_c`

Return <code><b>true</b></code> if address has specified order collateral type


<pre><code><b>fun</b> <a href="User.md#0xc0deb00c_User_exists_o_c">exists_o_c</a>&lt;B, Q, E&gt;(a: <b>address</b>): bool
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>fun</b> <a href="User.md#0xc0deb00c_User_exists_o_c">exists_o_c</a>&lt;B, Q, E&gt;(a: <b>address</b>): bool {<b>exists</b>&lt;<a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;&gt;(a)}
</code></pre>



</details>

<a name="0xc0deb00c_User_init_o_c"></a>

## Function `init_o_c`

Initialize order collateral container for given user, aborting
if already initialized


<pre><code><b>fun</b> <a href="User.md#0xc0deb00c_User_init_o_c">init_o_c</a>&lt;B, Q, E&gt;(user: &signer)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>fun</b> <a href="User.md#0xc0deb00c_User_init_o_c">init_o_c</a>&lt;B, Q, E&gt;(
    user: &signer,
) {
    // Assert user does not already have order collateral for market
    <b>assert</b>!(!<a href="User.md#0xc0deb00c_User_exists_o_c">exists_o_c</a>&lt;B, Q, E&gt;(s_a_o(user)), <a href="User.md#0xc0deb00c_User_E_O_C_EXISTS">E_O_C_EXISTS</a>);
    // Assert given market <b>has</b> actually been registered
    <b>assert</b>!(r_i_r&lt;B, Q, E&gt;(), <a href="User.md#0xc0deb00c_User_E_NO_MARKET">E_NO_MARKET</a>);
    // Pack empty order collateral container
    <b>let</b> o_c = <a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;{b_c: c_z&lt;B&gt;(), b_a: 0, q_c: c_z&lt;Q&gt;(), q_a: 0};
    <b>move_to</b>&lt;<a href="User.md#0xc0deb00c_User_OC">OC</a>&lt;B, Q, E&gt;&gt;(user, o_c); // Move <b>to</b> user account
}
</code></pre>



</details>