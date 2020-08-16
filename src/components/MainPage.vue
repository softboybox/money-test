<template>
    <div>
        <h1>Добавление платежной карты</h1>
        <div class="pay-box">
            <form class="form">
                <label for="card_number" v-if="isNumberError" class="form__error">Введите корректный номер карты</label>
                <input type="text" placeholder="Номер карты" id="card_number" @input="isNumber(); isCardHolder()"
                       v-mask="'#### #### #### ####'"
                       v-model="card_number" v-bind:class="{ error: isNumberError }"
                       :style="{ 'background-image': 'url(' + cardImg + ')' }" class="form__card-input">
                <div class="form__group">
                    <div class="form__label-group">
                        <input type="text" placeholder="Срок действия" id="expire_date"
                               @input="isNumber(); isValidExpireDate()"
                               v-mask="'##/##'" v-model="expire_date" v-bind:class="{ error: isExpireDateError }">
                        <label for="expire_date" v-if="isExpireDateError" class="form__error">Введите корректную
                            дату</label>
                    </div>
                    <div class="form__label-group">
                        <input type="text" placeholder="CVC код" id="cvv" v-bind:class="{ error: isCvvError }"
                               @input="isNumber()"
                               v-model="cvv_code" v-mask="'###'" style="-webkit-text-security: disc;">
                        <label for="cvv" v-if="isCvvError" class="form__error">Введите корректный CVC</label>
                    </div>
                </div>
                <p>Для привязки карты мы проведем платеж в размере 1.00 UAH, который будет взвращен в течении 30
                    минут</p>
                <button type="button" @click="validCheck" v-if="!spinner">Получить деньги</button>
                <div class="loader" v-if="spinner"></div>
            </form>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'MainPage',
        data() {
            return {
                card_number: '',
                expire_date: '',
                cvv_code: '',
                isNumberError: false,
                isExpireDateError: false,
                isCvvError: false,
                visaImg: 'image/visa.png',
                masterImg: 'image/master.png',
                cardImg: '',
                spinner: false
            }
        },
        methods: {
            isNumber: function ($event) {
                $event = ($event) ? $event : window.event;
                let charCode = ($event.which) ? $event.which : $event.keyCode;
                if ((charCode > 47 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
                    $event.preventDefault();
                } else {
                    return true;
                }
            },
            isCardHolder() {
                if (this.card_number.charAt(0) === '4') {
                    this.cardImg = this.visaImg;
                } else if (this.card_number.charAt(0) === '5') {
                    this.cardImg = this.masterImg;
                }
            },
            isValidCard(card_number) {
                let clearNumber = card_number.replace(/\s/g, '');
                if (/[^0-9-\s]+/.test(card_number) || !card_number || clearNumber.length !== 16) return false;

                let nCheck = 0, bEven = false;
                card_number = card_number.replace(/\D/g, "");

                for (let n = card_number.length - 1; n >= 0; n--) {
                    let cDigit = card_number.charAt(n),
                        nDigit = parseInt(cDigit, 10);

                    if (bEven && (nDigit *= 2) > 9) nDigit -= 9;

                    nCheck += nDigit;
                    bEven = !bEven;
                }

                return (nCheck % 10) == 0;
            },
            isValidExpireDate() {
                let month = this.expire_date.substring(0, 2);

                if (month.length == 2) {
                    let month_number = parseInt(month);
                    if (month_number >= 13) {
                        this.expire_date = this.expire_date.substring(0, 1)
                    } else {
                        return true;
                    }
                }
            },
            isExpiredDate() {
                let today = new Date();
                today.setHours(0, 0, 0, 0);
                let formate_date = '20' + this.expire_date.substring(3, 5) + '-' + this.expire_date.substring(0, 2);
                let card_date = new Date(formate_date);
                return card_date >= today;
            },
            isValidCvv() {
                return !!(this.cvv_code && this.cvv_code.length == 3);
            },
            payDone() {
                alert('Отправка успешна');
                this.spinner = false;
            },
            validCheck() {
                this.isNumberError = !this.isValidCard(this.card_number);
                this.isExpireDateError = !this.isExpiredDate();
                this.isCvvError = !this.isValidCvv();
                this.spinner = !(this.isNumberError || this.isExpireDateError || this.isCvvError);
                if (this.spinner) {
                    setTimeout(this.payDone, 3000);
                }
            }
        }
    }
</script>
<style lang="scss">
   @font-face {
        font-family: "CoreSans";
        src: url("../assets/fonts/CoreSansG-ExtraLight.ttf");
    }
    body{
        font-family: "CoreSans" !important;
    }
    .pay-box {
        border: 1px solid #E4E4E4;
        box-sizing: border-box;
        border-radius: 4px;
        padding: 16px;
        @media screen and (min-width: 1320px) {
            padding: 49px 278px;
        }
    }

    .form {
        font-style: normal;
        font-weight: normal;
        font-size: 16px;
        line-height: 17px;
        color: #808080;

        input {
            padding: 20px 16px;
            width: 100%;
            box-sizing: border-box;
            border: 1px solid #CCCCCC;
            border-radius: 4px;
            margin-bottom: 16px;
            outline: none;
        }

        &__card-input {
            background-position: right 14px center;
            background-repeat: no-repeat;
            background-size: 35px;
        }

        &__error {
            font-style: normal;
            font-weight: normal;
            font-size: 12px;
            line-height: 20px;
            color: #FF8D8D;
        }

        &__label-group {
            display: flex;
            flex-direction: column;
            width: 49%;

            label {
                text-align: right;
            }

            input {
                margin-bottom: 0;
            }
        }

        p {
            font-style: normal;
            font-weight: normal;
            font-size: 16px;
            line-height: 20px;
            color: #808080;
        }

        button {
            background: linear-gradient(122.5deg, #5A4BE6 -33.07%, #73AFF7 48.35%, #93D0D9 139.94%);
            border-radius: 4px;
            color: white;
            font-size: 16px;
            border: none;
            box-sizing: border-box;
            width: 100%;
            height: 56px;
            margin-top: 15px;
            cursor: pointer;
        }

        &__group {
            display: flex;
            justify-content: space-between;
        }

        .error {
            border: 1px solid #FF8D8D;
        }
    }

    .loader,
    .loader:before,
    .loader:after {
        border-radius: 50%;
        width: 2.5em;
        height: 2.5em;
        -webkit-animation-fill-mode: both;
        animation-fill-mode: both;
        -webkit-animation: load7 1.8s infinite ease-in-out;
        animation: load7 1.8s infinite ease-in-out;
    }

    .loader {
        color: linear-gradient(122.5deg, #5A4BE6 -33.07%, #73AFF7 48.35%, #93D0D9 139.94%);;
        font-size: 3px;
        margin: 10px auto;
        position: relative;
        text-indent: -9999em;
        -webkit-transform: translateZ(0);
        -ms-transform: translateZ(0);
        transform: translateZ(0);
        -webkit-animation-delay: -0.16s;
        animation-delay: -0.16s;
    }

    .loader:before,
    .loader:after {
        content: '';
        position: absolute;
        top: 0;
    }

    .loader:before {
        left: -3.5em;
        -webkit-animation-delay: -0.32s;
        animation-delay: -0.32s;
    }

    .loader:after {
        left: 3.5em;
    }

    @-webkit-keyframes load7 {
        0%,
        80%,
        100% {
            box-shadow: 0 2.5em 0 -1.3em;
        }
        40% {
            box-shadow: 0 2.5em 0 0;
        }
    }

    @keyframes load7 {
        0%,
        80%,
        100% {
            box-shadow: 0 2.5em 0 -1.3em;
        }
        40% {
            box-shadow: 0 2.5em 0 0;
        }
    }
</style>