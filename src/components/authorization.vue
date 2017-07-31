<template>
<div class="authorization">
    <h2>Sign In</h2>
    <div class="auth-form-body mt-3">

        <label for="login_field">
            Username or email address
        </label>
        <input class="form-control input-block" id="login_field" name="login" tabindex="1" type="text" />

        <label for="password">
            Password <a  class="label-link">Forgot password?</a>
        </label>
        <input class="form-control form-control input-block" id="password" name="password" tabindex="2" type="password" />

        <input class="btn btn-primary btn-block" data-disable-with="Signing in…" name="commit" tabindex="3" type="submit" value="Sign in" />
    </div>

</div>


</template>