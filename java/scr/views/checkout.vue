<template>
  <div class="checkout">
    <div class="container">
      <h1 class="page-title">Оформление заказа</h1>
      
      <div v-if="cartItems.length === 0" class="empty-cart">
        <p>Ваша корзина пуста. Добавьте товары, чтобы оформить заказ.</p>
        <router-link to="/menu" class="cta-button">
          Перейти в меню
        </router-link>
      </div>
      
      <div v-else class="checkout-content">
        <div class="checkout-form">
          <h2>Данные для доставки</h2>
          <form @submit.prevent="submitOrder">
            <div class="form-group">
              <label for="name">Имя *</label>
              <input 
                type="text" 
                id="name" 
                v-model="form.name" 
                required
                :class="{ 'error': errors.name }"
              />
              <span v-if="errors.name" class="error-text">{{ errors.name }}</span>
            </div>
            
            <div class="form-group">
              <label for="phone">Телефон *</label>
              <input 
                type="tel" 
                id="phone" 
                v-model="form.phone" 
                required
                :class="{ 'error': errors.phone }"
              />
              <span v-if="errors.phone" class="error-text">{{ errors.phone }}</span>
            </div>
            
            <div class="form-group">
              <label for="address">Адрес доставки *</label>
              <textarea 
                id="address" 
                v-model="form.address" 
                rows="3"
                required
                :class="{ 'error': errors.address }"
              ></textarea>
              <span v-if="errors.address" class="error-text">{{ errors.address }}</span>
            </div>
            
            <div class="form-group">
              <label for="notes">Примечания к заказу</label>
              <textarea 
                id="notes" 
                v-model="form.notes" 
                rows="3"
                placeholder="Например: позвонить за 15 минут до доставки"
              ></textarea>
            </div>
            
            <button type="submit" class="submit-btn" :disabled="isSubmitting">
              {{ isSubmitting ? 'Оформляем...' : 'Подтвердить заказ' }}
            </button>
          </form>
        </div>
        
        <div class="order-summary">
          <h2>Ваш заказ</h2>
          <div class="order-items">
            <div 
              v-for="item in cartItems"