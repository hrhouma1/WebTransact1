
- Haifeng dispose de ces deux entités : 


```java
package com.eazybytes.cards.model;

import java.util.List;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;
// import lombok.*;
import jakarta.persistence.Table;


@Entity
@Table(name = "customer")
public class Customer {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "customer_id")
	private int customerId;
	
	@Column(name = "name")
	private String name;

	@OneToMany(mappedBy = "customer", cascade = CascadeType.ALL)
	private List<Cards> cards;

	public Customer() {
	}

	
	public Customer(String name) {
		this.name = name;
	}



	public int getCustomerId() {
		return customerId;
	}

	public void setCustomerId(int customerId) {
		this.customerId = customerId;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public List<Cards> getCards() {
		return cards;
	}

	public void setCards(List<Cards> cards) {
		this.cards = cards;
	}
		
}

   ```




```java
package com.eazybytes.cards.model;
import java.time.LocalDate;

import com.fasterxml.jackson.annotation.JsonIgnore;

import jakarta.persistence.*;


@Entity
@Table(name = "cards")
public class Cards {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "card_id")
	private int cardId;

	@Column(name = "card_number")
	private String cardNumber;

	@Column(name = "card_type")
	private String cardType;

	@Column(name = "total_limit")
	private int totalLimit;

	@Column(name = "amount_used")
	private int amountUsed;

	@Column(name = "available_amount")
	private int availableAmount;

	@Column(name = "create_dt")
	private LocalDate createDt;

	/*@ManyToOne
	@JoinColumn(name = "customer_id", nullable = false)*/

	@ManyToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "customer_id", nullable = false)
	@JsonIgnore
	private Customer customer;
	

	public Cards() {
	}

	public Cards(String cardNumber, String cardType, int totalLimit, int amountUsed, int availableAmount,
			LocalDate createDt, Customer customer) {

		this.cardNumber = cardNumber;
		this.cardType = cardType;
		this.totalLimit = totalLimit;
		this.amountUsed = amountUsed;
		this.availableAmount = availableAmount;
		this.createDt = createDt;
		this.customer = customer;
	}

	public int getCardId() {
		return cardId;
	}

	public void setCardId(int cardId) {
		this.cardId = cardId;
	}

	public String getCardNumber() {
		return cardNumber;
	}

	public void setCardNumber(String cardNumber) {
		this.cardNumber = cardNumber;
	}

	public String getCardType() {
		return cardType;
	}

	public void setCardType(String cardType) {
		this.cardType = cardType;
	}

	public int getTotalLimit() {
		return totalLimit;
	}

	public void setTotalLimit(int totalLimit) {
		this.totalLimit = totalLimit;
	}

	public int getAmountUsed() {
		return amountUsed;
	}

	public void setAmountUsed(int amountUsed) {
		this.amountUsed = amountUsed;
	}

	public int getAvailableAmount() {
		return availableAmount;
	}

	public void setAvailableAmount(int availableAmount) {
		this.availableAmount = availableAmount;
	}

	public LocalDate getCreateDt() {
		return createDt;
	}

	public void setCreateDt(LocalDate createDt) {
		this.createDt = createDt;
	}

	public Customer getCustomer() {
		return customer;
	}

	public void setCustomer(Customer customer) {
		this.customer = customer;
	}
}
   ```
