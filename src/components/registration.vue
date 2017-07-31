<template>
    <div class = "container registration-body">
        <div class="registration">
        <div class="registration_cont">
          
           <br>
            <h2>Registration</h2>
            <br>
            <table>
                <tr>
                    <td class='left_col'>
                       <div class="try">
                        <label class="lbl"><p>NickName</p></label></div>
                    </td>
                    <td >
                        <div class="login">
                            <input type="text" maxlength="16" name="NickName" v-model="newUser.name" placeholder="Login" > 
                            <br>
                                <span class="error_control" v-show="isErrorName">>{{ userMsg  }}
                                </span>
                        </div>
                    </td>
                </tr>
                <tr>
                    <td class='left_col'>
                        <label class="lbl"><p>Email</p></label>
                    </td>
                    <td>
                           <div class="email">
                               <input type="email" name="Email" v-model="newUser.email" placeholder="Email"><br>
                               <span v-show="isErrorMail" >{{ emailMsg }}</span>
                           </div>
                    </td>
                    
                </tr>
                <tr>
                    <td class='left_col'>
                        <label class="lbl"><p>City</p></label>
                    </td>
                    <td>
                        <div class="select">
                            <select name="city" v-model="newUser.city">
                                <option value="" disabled selected hidden>Select city</option>
                                <option>Брест</option>
                                <option>Витебск</option>
                                <option>Гомель</option>
                                <option>Гродно</option>
                                <option>Минск</option>
                            </select>
                        </div>
                    </td>
                </tr>
                <tr>
                    <td class='left_col'>
                        <label class="lbl"><p>Password</p></label> 
                    </td>
                    <td>
                        <div class="password">
                            <input type="password" name="Password" v-model="newUser.password" placeholder="Password" maxlength="16" >
                        </div>
                            <br>
                        <span class="error_control" v-show="isErrorPswd">{{ passwordMsg }}</span>
                    </td>
                </tr>
                <tr>
                   <td class='left_col'>
                        <label class="lbl"><p>Confirm password</p></label> 
                   </td>
                    <td>
                        <div class="password conf">
                            <input type="password" name="Password2" v-model="newUser.password2" placeholder="Repeat password" maxlength="16">
                            <br>
                            <span class="error_control" v-show="isErrorPswd">{{ passwordMsg2 }}</span>
                        </div>
                    </td>
                     
                </tr>
                <tr class="checkbox_tr">
                   <td>
                    <div class="checkbox">
                            <p>I want to receive newsletters about new vacancies</p>
                    </div>
                    </td>
                    <td>
                        <input type="checkbox" class = "chbox" id="chbox" v-model="newUser.mailing">
                    </td>
                </tr>
                <tr>
                    <button :class="{active: disableBtn}" :disabled="disableBtn" @click.prevent="on_signup">
                        Sign Up
                    </button>   
                </tr>
   
            </table>     
        </div>
    </div>
    </div>
    
</template>

<script>
    export default {
        name: "registration",
        data() {
            return {
                newUser: {
                    name: '',
                    email: '',
                    city: '',
                    password: '',
                    password2: '',
                    mailing: false,
                },
                // style: {
                //     active: true,
                //     'text-danger': false
                // },
                isErrorName: true,
                isErrorMail: true,
                isErrorPswd: true,
                userMsg: '',
                emailMsg: '',
                passwordMsg: '',
                passwordMsg2: '',
                disableBtn: true,
                isActive: true,
                hasError: false
            }
        },



        watch: {
            'newUser.email': function(value) {
                console.log(value);
                if(this.requiredField(value,'emailMsg')){
                    this.valid_email(value, 'emailMsg');
                }
            },
            'newUser.password': function (value) {
                console.log(value);
                if(this.requiredField(value, 'passwordMsg'))
                    if (this.check_password_length(value, 'passwordMsg', 16)) {
                        this.check_passwords_match();
                        console.log(this.disableBtn);
                    }
            },
            'newUser.password2': function (value) {
                console.log(value);
                if(this.requiredField(value, 'passwordMsg2'))
                    if (this.check_password_length(value, 'passwordMsg2', 16)) {
                        this.check_passwords_match();
                        console.log(this.disableBtn);
                        console.log(this.isErrorName);
                    }

            },
            'newUser.name': function (value) {
                this.requiredField(value,'userMsg');

            }
        },

        methods: {

            requiredField(value, msg){
                if(value !== ''){
                    this[msg] = '';
                    return true;
                }
                else{
                    this[msg]='Field is required';
                    return false;
                }
            },


            valid_email(email, msg) {
                let emailRE = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                if (emailRE.test(this.newUser.email)) {
                    this[msg] = '';
                    return true;
                } else {
                    this[msg] = 'Keep typing...waiting for a valid email';
                    return false;
                }
            },

            check_password_length(value, msg, total) {
                let length = value.length;
                let sum = 0;
                let display;

                sum = (total - length);

                switch (sum) {
                    case 0:
                        display = '';
                        break;
                    case 1:
                        display = 'Keep going - just need ' + sum + ' more character.';
                        break;
                    default:
                        display = 'Keep going - just need ' + sum + ' more characters';
                }

                //ukgjygkuyfkutfhtchkgcv
                if (length >= total) {
                    this[msg] = '';
                    return true;
                } else {
                    this[msg] = display;
                    return false;
                }

            },

            check_passwords_match() {
                if (this.newUser.password.length > 15 && this.newUser.password2.length > 15) {
                    if (this.newUser.password2 !== this.newUser.password) {
                        console.log(this.newUser.password);
                        this.passwordMsg2  = 'Passwords do not match';
                        this.disableBtn = true;
                        return true;
                    } else {
                        this.password2_msg = '';
                        this.disableBtn = false;
                        return false;
                    }
                }
            },
            //            isValid(){
            //                if(this.newUser.name !== '' && this.newUser.email !== '' && this.newUser.city !== '' && this.newUser.password !== '' && this.newUser.password2 !== '')
            //                {
            //                    this.error_msg = true;
            //                    this.user_msg = 'NOO';
            //                }
            //                else {
            //                    this.error_msg = false;
            //                    this.user_msg = '';
            //                }
            //            },

            check_requered_fields() {
                this.disableBtn = this.newUser.name !== '' && this.newUser.email !== '' && this.newUser.city !== '' && this.newUser.password !== '' && this.newUser.password2 !== '';

            },
            on_signup(){
                if(this.newUser.name !== '' && this.newUser.email !== '' && this.newUser.city !== '' && this.newUser.password !== '' && this.newUser.password2 !== '') {
                    console.log(this.newUser.name);
                    console.log(this.newUser.email);
                    console.log(this.newUser.city);
                    console.log(this.newUser.password);
                    console.log(this.newUser.mailing);
                    axios.post('http://jsonplaceholder.typicode.com/posts', {
                        name: this.newUser.name,
                        email: this.newUser.email,
                        city: this.newUser.city,
                        password: this.newUser.password,
                        mailing: this.newUser.mailing
                    }).then(function (response) {
                        console.log(response);
                    })
                        .catch(function (error) {
                            console.log(error);
                        });
                }
                else{
                    console.log('не все поля заполнены');
                }

            }
        }
    }

</script>

<style scoped>
    * {
        background-color: #039BE5;
        color: white;
       
        margin: auto;
        
    }

    table{
        table-layout: fixed;
        width: 40%;
    }
    p {
        color: black
    }
    
    tr{
        line-height: 0px;
        
        
        
    }
    
    td {
        border: 1px solid black;
        text-align: left;
        vertical-align: middle;
        
    }
    
    .container {
        background: #039BE5;
        width: 900px;
        height: 300px;
        margin: 200px auto;
        text-align: center;
        border-radius: 15px;
        color: red;
        box-shadow: 1px 2px 4px 2px rgba(0,0,0,0.15);
    }

    
    input {
        border-radius: 5px;
        width: 250px;
        margin-top: 0px;
    }
    
    
    
    select {
        width: 250px;
        text-align: center;
        border-radius: 5px;
        height: 26px;
    }
    

    
    button {
        color: white;
        background-color: indigo;
        width: 300px;
        border-radius: 5px;
        height: 35px;
    }
    
    
    select {
        display: inline-block;
    }

    td{
        width: 250px;
        border: none;
        
    }
    
    .left_col {
        width: 130px;
        border-bottom: 1px solid white;
    }
    

    .checkbox{
        display: block;
        margin-bottom: 10px;
        margin-top: 10px;
        width: 470px;
        padding-bottom: 10px;
        vertical-align: top;
            
        
    }

    

    /*попробовать стиль к валидации на ввод всех полей
    (нужна будет какая то булевская переменная которая будет смотреть за заполненностью и если все плохо поля подсвечиваются
     красным, иначе все нормас)
     вопрос может сделать при нажатии на кнопку???? или динамическое отображение)
     возможно динамическая нормализация полей??? */

</style>
