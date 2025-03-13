---
title: Купить билет
layout: default
---

## Покупка билетов

Выберите игру и оплатите билет через Stripe.

<form action="/charge" method="post" id="payment-form">
  <div class="form-row">
    <label for="card-element">
      Кредитная или дебетовая карта
    </label>
    <div id="card-element">
      <!-- Элемент карты Stripe будет вставлен сюда -->
    </div>

    <!-- Сообщения об ошибках будут отображаться здесь -->
    <div id="card-errors" role="alert"></div>
  </div>

  <button type="submit">Оплатить</button>
</form>

<script src="https://js.stripe.com/v3/"></script>
<script>
  var stripe = Stripe('YOUR_STRIPE_PUBLIC_KEY');
  var elements = stripe.elements();
  var card = elements.create('card');
  card.mount('#card-element');

  var form = document.getElementById('payment-form');
  form.addEventListener('submit', function(event) {
    event.preventDefault();
    stripe.createToken(card).then(function(result) {
      if (result.error) {
        var errorElement = document.getElementById('card-errors');
        errorElement.textContent = result.error.message;
      } else {
        var hiddenInput = document.createElement('input');
        hiddenInput.setAttribute('type', 'hidden');
        hiddenInput.setAttribute('name', 'stripeToken');
        hiddenInput.setAttribute('value', result.token.id);
        form.appendChild(hiddenInput);
        form.submit();
      }
    });
  });
</script>
