<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DHL Tracking Portal</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      color: #333;
      min-height: 100vh;
      background: #ffffff;
    }

    .page-section {
      display: none;
    }

    .page-section.active {
      display: block;
    }

    .login-section,
    .tracking-section {
      min-height: 100vh;
      position: relative;
    }

    .background-shape {
      position: absolute;
      border-radius: 50%;
      opacity: 0.05;
      filter: blur(40px);
      display: none;
    }

    .shape1 {
      width: 300px;
      height: 300px;
      background: #ffb800;
      top: -100px;
      left: -100px;
      animation: float 6s ease-in-out infinite;
    }

    .shape2 {
      width: 200px;
      height: 200px;
      background: #ffb800;
      bottom: -50px;
      right: -50px;
      animation: float 8s ease-in-out infinite reverse;
    }

    @keyframes float {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(20px); }
    }

    .login-header-bar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      background: #ffb800;
      padding: 20px 30px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
      z-index: 1000;
      display: flex;
      align-items: center;
    }

    .dhl-logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .dhl-logo svg {
      width: 100px;
      height: auto;
    }

    .header-title {
      margin-left: 30px;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .header-title h2 {
      color: #333;
      font-size: 18px;
      font-weight: 700;
      margin: 0;
      padding: 0;
    }

    .header-title p {
      color: #333;
      font-size: 12px;
      margin: 5px 0 0 0;
      padding: 0;
    }

    .login-container {
      width: 100%;
      max-width: 450px;
      padding: 40px;
      background: #ffffff;
      border: 1px solid #e0e0e0;
      border-radius: 4px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
      z-index: 10;
      animation: slideIn 0.6s ease-out;
      margin: 140px auto 60px;
      position: relative;
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .login-header {
      text-align: center;
      margin-bottom: 40px;
    }

    .login-header h1 {
      color: #333;
      font-size: 28px;
      margin-bottom: 10px;
      font-weight: 700;
    }

    .login-header p {
      color: #666;
      font-size: 14px;
      margin-bottom: 5px;
    }

    .login-header .subtitle {
      color: #999;
      font-size: 12px;
    }

    .form-group {
      margin-bottom: 25px;
    }

    label {
      display: block;
      color: #333;
      font-size: 14px;
      font-weight: 500;
      margin-bottom: 10px;
    }

    .input-wrapper {
      position: relative;
      display: flex;
      align-items: center;
    }

    .input-icon {
      position: absolute;
      left: 15px;
      color: #999;
      font-size: 18px;
      z-index: 2;
    }

    input {
      width: 100%;
      padding: 15px 15px 15px 45px;
      border: 1px solid #ddd;
      border-radius: 4px;
      background: #fff;
      color: #333;
      font-size: 16px;
      transition: all 0.3s ease;
    }

    input::placeholder {
      color: #bbb;
    }

    input:focus {
      outline: none;
      background: #fafafa;
      border-color: #ffb800;
      box-shadow: 0 0 5px rgba(255, 184, 0, 0.2);
    }

    input.error {
      border-color: #ff6b6b;
      background: rgba(255, 107, 107, 0.05);
    }

    input.error:focus {
      box-shadow: 0 0 5px rgba(255, 107, 107, 0.3);
    }

    .error-message {
      color: #ff6b6b;
      font-size: 12px;
      margin-top: 8px;
      display: none;
    }

    .error-message.show {
      display: block;
    }

    .login-btn {
      width: 100%;
      padding: 15px;
      margin-top: 30px;
      border: none;
      border-radius: 12px;
      background: linear-gradient(135deg, #ffb800 0%, #ffa500 100%);
      color: #333;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
    }

    .login-btn:hover:not(:disabled) {
      transform: translateY(-2px);
      box-shadow: 0 10px 25px rgba(255, 184, 0, 0.4);
    }

    .login-btn:active:not(:disabled) {
      transform: translateY(0);
    }

    .login-btn:disabled {
      opacity: 0.8;
      cursor: not-allowed;
    }

    .spinner {
      display: none;
      width: 20px;
      height: 20px;
      border: 3px solid rgba(51, 51, 51, 0.3);
      border-top: 3px solid #333;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
      margin-right: 10px;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .btn-content {
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .spinner.show {
      display: inline-block;
    }

    .response-message {
      text-align: center;
      color: #4caf50;
      font-size: 14px;
      margin-top: 15px;
      display: none;
    }

    .response-message.show {
      display: block;
    }

    .tracking-section {
      background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
      padding-bottom: 40px;
    }

    .tracking-header {
      background: linear-gradient(135deg, #007bff, #0056b3);
      color: #fff;
      text-align: center;
      padding: 20px;
      font-size: 1.8rem;
      font-weight: 600;
      letter-spacing: 1px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    .container {
      max-width: 1100px;
      margin: 30px auto;
      background: #fff;
      border-radius: 16px;
      padding: 30px;
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
    }

    .section {
      margin-bottom: 35px;
    }

    .section-title {
      font-size: 1.3rem;
      font-weight: 600;
      color: #007bff;
      border-left: 4px solid #007bff;
      padding-left: 12px;
      margin-bottom: 15px;
      text-transform: uppercase;
    }

    .delivery-info,
    .address,
    .tracking,
    .popup-footer,
    .payment-review {
      border-radius: 12px;
    }

    .delivery-info {
      background: linear-gradient(135deg, #f0f8ff, #e1f0ff);
      border-radius: 12px;
      padding: 20px;
      line-height: 1.7;
      border: 1px solid #cce5ff;
      box-shadow: 0 2px 8px rgba(0, 123, 255, 0.1);
    }

    .meta {
      display: flex;
      justify-content: space-between;
      font-size: 0.9rem;
      margin-top: 10px;
      color: #555;
      flex-wrap: wrap;
    }

    .meta span {
      margin-bottom: 5px;
    }

    .products {
      display: flex;
      overflow-x: auto;
      gap: 15px;
      scrollbar-width: thin;
      padding-bottom: 10px;
    }

    .products::-webkit-scrollbar {
      height: 6px;
    }

    .products::-webkit-scrollbar-track {
      background: #f1f1f1;
      border-radius: 10px;
    }

    .products::-webkit-scrollbar-thumb {
      background: #007bff;
      border-radius: 10px;
    }

    .product-card {
      background: linear-gradient(135deg, #f9f9f9, #f0f0f0);
      border: 1px solid #e0e0e0;
      border-radius: 12px;
      flex: 0 0 180px;
      text-align: center;
      padding: 12px;
      transition: all 0.3s ease;
    }

    .product-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
      border-color: #007bff;
    }

    .product-card img {
      width: 100%;
      max-height: 90px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 8px;
    }

    .product-name {
      font-size: 0.9rem;
      font-weight: 500;
      margin-bottom: 5px;
    }

    .product-detail,
    .product-info {
      font-size: 0.85rem;
      color: #666;
    }

    .product-info {
      margin-top: 8px;
      color: #ff0000;
    }

    .button-link {
      background-color: #007bff;
      border: none;
      color: white;
      padding: 15px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      cursor: pointer;
      border-radius: 10px;
      margin-top: 20px;
    }

    .address {
      background: linear-gradient(135deg, #fafafa, #f0f0f0);
      border-radius: 12px;
      padding: 20px;
      line-height: 1.7;
      border: 1px solid #e0e0e0;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    }

    .tracking {
      background: linear-gradient(135deg, #f8f9ff, #e8f0ff);
      border-radius: 16px;
      padding: 25px 30px;
      border: 1px solid #cce5ff;
      position: relative;
      box-shadow: 0 4px 12px rgba(0, 123, 255, 0.1);
    }

    .tracking-container {
      display: flex;
      justify-content: space-between;
      position: relative;
      margin: 30px 0 40px;
      gap: 10px;
      flex-wrap: wrap;
    }

    .tracking-step {
      display: flex;
      flex-direction: column;
      align-items: center;
      flex: 1;
      position: relative;
      z-index: 2;
      min-width: 120px;
    }

    .tracking-step:not(:last-child)::after {
      content: '';
      position: absolute;
      top: 25px;
      left: 50%;
      width: 100%;
      height: 4px;
      background: #e0e0e0;
      z-index: -1;
    }

    .tracking-step.active:not(:last-child)::after {
      background: linear-gradient(90deg, #007bff, #00b4d8);
      animation: fillProgress 0.5s ease-in-out;
    }

    .tracking-step.completed:not(:last-child)::after {
      background: #007bff;
    }

    .tracking-icon {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 10px;
      background: #e0e0e0;
      color: #777;
      font-size: 1.2rem;
      transition: all 0.3s ease;
    }

    .tracking-step.active .tracking-icon {
      background: linear-gradient(135deg, #007bff, #00b4d8);
      color: white;
      box-shadow: 0 0 0 8px rgba(0, 123, 255, 0.2);
    }

    .tracking-step.completed .tracking-icon {
      background: linear-gradient(135deg, #007bff, #007bff);
      color: white;
    }

    .tracking-label {
      font-size: 0.85rem;
      font-weight: 500;
      text-align: center;
      color: #777;
      max-width: 100px;
    }

    .tracking-step.active .tracking-label,
    .tracking-step.completed .tracking-label {
      color: #007bff;
      font-weight: 600;
    }

    .tracking-time {
      font-size: 0.75rem;
      color: #999;
      margin-top: 5px;
      text-align: center;
    }

    .tracking-step.active .tracking-label {
      margin-top: 25px;
      font-size: 1rem;
      text-align: center;
      font-weight: 500;
      padding: 12px;
      border-radius: 10px;
      background: linear-gradient(135deg, #e8f3ff, #d9ebff);
      color: #007bff;
      border-left: 4px solid #007bff;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    @keyframes fillProgress {
      from { width: 0%; }
      to { width: 100%; }
    }

    .ads {
      background: linear-gradient(90deg, #007bff, #00b4d8);
      color: #fff;
      padding: 15px 20px;
      border-radius: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      animation: fadeText 10s infinite;
      position: relative;
      box-shadow: 0 4px 12px rgba(0, 123, 255, 0.2);
      gap: 10px;
      flex-wrap: wrap;
    }

    .ads p {
      flex: 1;
      margin-right: 10px;
      font-weight: 500;
      font-size: 0.95rem;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .ads button {
      background: #fff;
      color: #007bff;
      border: none;
      padding: 10px 18px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .ads button:hover {
      background: #e6f0ff;
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
    }

    @keyframes fadeText {
      0%, 33% { opacity: 1; }
      33.1%, 66% { opacity: 0; }
      66.1%, 100% { opacity: 1; }
    }

    .popup-overlay {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.7);
      display: none;
      justify-content: center;
      align-items: flex-end;
      z-index: 1000;
    }

    .popup {
      background: #fff;
      border-radius: 20px 20px 0 0;
      width: 100%;
      max-height: 90vh;
      padding: 30px;
      transform: translateY(100%);
      transition: transform 0.4s ease;
      overflow-y: auto;
      box-shadow: 0 -5px 25px rgba(0, 0, 0, 0.2);
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
    }

    .popup.show {
      transform: translateY(0);
    }

    .popup-header {
      font-size: 1.5rem;
      font-weight: 600;
      color: #007bff;
      margin-bottom: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-bottom: 15px;
      border-bottom: 2px solid #e8f3ff;
    }

    .popup-close {
      background: none;
      border: none;
      font-size: 1.8rem;
      cursor: pointer;
      color: #007bff;
      line-height: 1;
      transition: all 0.3s;
    }

    .popup-close:hover {
      transform: scale(1.1);
      color: #0056b3;
    }

    .popup-content {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }

    .popup-item {
      display: flex;
      justify-content: space-between;
      margin-bottom: 15px;
      padding-bottom: 10px;
      border-bottom: 1px dashed #e0e0e0;
    }

    .popup-label {
      font-weight: 600;
      color: #555;
      font-size: 0.95rem;
    }

    .popup-value {
      font-size: 1rem;
      text-align: right;
      color: #333;
    }

    .popup-products {
      grid-column: 1 / -1;
      margin-top: 10px;
    }

    .popup-products-title {
      font-size: 1.2rem;
      font-weight: 600;
      color: #007bff;
      margin-bottom: 15px;
      padding-bottom: 8px;
      border-bottom: 2px solid #e8f3ff;
    }

    .popup-product {
      display: flex;
      align-items: center;
      padding: 12px;
      border-radius: 10px;
      background: linear-gradient(135deg, #f9f9f9, #f0f0f0);
      margin-bottom: 10px;
      transition: all 0.3s;
    }

    .popup-product:hover {
      transform: translateX(5px);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .popup-product img {
      width: 60px;
      height: 60px;
      object-fit: cover;
      border-radius: 8px;
      margin-right: 15px;
      border: 2px solid #e0e0e0;
    }

    .popup-product-info {
      flex: 1;
    }

    .popup-product-name {
      font-weight: 500;
      margin-bottom: 5px;
    }

    .popup-product-detail {
      font-size: 0.85rem;
      color: #666;
    }

    .popup-footer {
      margin-top: 25px;
      padding-top: 15px;
      border-top: 2px solid #e8f3ff;
      text-align: center;
      font-size: 0.9rem;
      color: #666;
      background: linear-gradient(135deg, #f8f9ff, #e8f0ff);
      padding: 15px;
      border-radius: 10px;
    }

    .payment-review {
      text-align: center;
      margin: 40px 0 30px;
      padding: 25px;
      background: linear-gradient(135deg, #e8f3ff, #d9ebff);
      border-radius: 14px;
      border: 2px solid #007bff;
      box-shadow: 0 4px 15px rgba(0, 123, 255, 0.2);
    }

    .payment-review-btn {
      background: linear-gradient(135deg, #007bff, #0062cc);
      color: #fff;
      border: none;
      padding: 16px 32px;
      border-radius: 12px;
      font-weight: 700;
      font-size: 1.05rem;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 6px 20px rgba(0, 123, 255, 0.35);
      display: inline-flex;
      align-items: center;
      gap: 12px;
      letter-spacing: 0.5px;
    }

    .payment-review-btn:hover {
      transform: translateY(-4px);
      box-shadow: 0 10px 30px rgba(0, 123, 255, 0.45);
      background: linear-gradient(135deg, #0056b3, #004085);
    }

    .payment-review-btn:active {
      transform: translateY(-1px);
      box-shadow: 0 4px 15px rgba(0, 123, 255, 0.35);
    }

    .footer {
      text-align: center;
      font-size: 0.85rem;
      color: #666;
      margin-top: 20px;
      padding-top: 20px;
      border-top: 1px solid #e0e0e0;
    }

    .footer span {
      color: #007bff;
      font-weight: 600;
    }

    .footer-links {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 25px;
      padding-top: 25px;
      border-top: 1px solid #e0e0e0;
    }

    .footer-links a {
      color: #666;
      text-decoration: none;
      font-size: 12px;
      transition: color 0.3s ease;
    }

    .footer-links a:hover {
      color: #ffb800;
    }

    .css-icon {
      display: inline-block;
      margin-right: 8px;
      vertical-align: middle;
    }

    .warning-icon {
      width: 18px;
      height: 18px;
      background: #007bff;
      border-radius: 50%;
      position: relative;
    }

    .warning-icon::before {
      content: "!";
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-weight: bold;
      color: #fff;
      font-size: 12px;
    }

    .package-icon {
      width: 18px;
      height: 18px;
      background: #007bff;
      border-radius: 3px;
      position: relative;
    }

    .package-icon::before {
      content: "";
      position: absolute;
      top: 4px;
      left: 4px;
      right: 4px;
      bottom: 4px;
      border: 1px solid #fff;
      border-radius: 1px;
    }

    .idea-icon {
      width: 18px;
      height: 18px;
      background: #17a2b8;
      border-radius: 50%;
      position: relative;
    }

    .idea-icon::before {
      content: "";
      position: absolute;
      top: 5px;
      left: 5px;
      width: 8px;
      height: 8px;
      background: #fff;
      border-radius: 50%;
    }

    .idea-icon::after {
      content: "";
      position: absolute;
      bottom: 3px;
      left: 50%;
      transform: translateX(-50%);
      width: 0;
      height: 0;
      border-left: 3px solid transparent;
      border-right: 3px solid transparent;
      border-top: 4px solid #fff;
    }

    .shipping-icon {
      width: 20px;
      height: 12px;
      background: #007bff;
      position: relative;
      border-radius: 2px;
    }

    .shipping-icon::before {
      content: "";
      position: absolute;
      top: -4px;
      left: 2px;
      width: 16px;
      height: 6px;
      background: #007bff;
      border-radius: 3px 3px 0 0;
    }

    .shipping-icon::after {
      content: "";
      position: absolute;
      bottom: -2px;
      left: 3px;
      width: 4px;
      height: 4px;
      background: #333;
      border-radius: 50%;
      box-shadow: 7px 0 0 #333;
    }

    .shipping-icon-large {
      width: 24px;
      height: 16px;
      background: currentColor;
      position: relative;
      border-radius: 2px;
    }

    .shipping-icon-large::before {
      content: "";
      position: absolute;
      top: -4px;
      left: 2px;
      width: 20px;
      height: 6px;
      background: currentColor;
      border-radius: 3px 3px 0 0;
    }

    .shipping-icon-large::after {
      content: "";
      position: absolute;
      bottom: -2px;
      left: 4px;
      width: 4px;
      height: 4px;
      background: #333;
      border-radius: 50%;
      box-shadow: 9px 0 0 #333;
    }

    .plane-icon {
      width: 24px;
      height: 24px;
      position: relative;
    }

    .plane-icon::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 24px;
      height: 12px;
      background: currentColor;
      clip-path: polygon(0 50%, 100% 0, 100% 100%);
    }

    .plane-icon::after {
      content: "";
      position: absolute;
      bottom: 0;
      left: 8px;
      width: 8px;
      height: 8px;
      background: currentColor;
      border-radius: 50%;
    }

    .check-icon {
      width: 24px;
      height: 24px;
      position: relative;
    }

    .check-icon::before {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%) rotate(45deg);
      width: 12px;
      height: 6px;
      border-bottom: 3px solid currentColor;
      border-right: 3px solid currentColor;
    }

    .popup-item span:last-child {
      text-align: right;
    }

    .popup-products .popup-product {
      display: flex;
      align-items: center;
      padding: 12px;
      border-radius: 10px;
      background: linear-gradient(135deg, #f9f9f9, #f0f0f0);
      margin-bottom: 10px;
      transition: all 0.3s;
    }

    .popup-products .popup-product img {
      width: 60px;
      height: 60px;
      object-fit: cover;
      border-radius: 8px;
      margin-right: 15px;
      border: 2px solid #e0e0e0;
    }

    .popup-products .popup-product-info {
      flex: 1;
    }

    .popup-products .popup-product-na6 {
      color: #0000ff;
    }

    .tracking-message {
      display: flex;
      align-items: center;
      gap: 10px;
      color: #555;
      font-size: 1rem;
      margin-top: 20px;
    }

    .tracking-message .css-icon {
      margin: 0;
    }

    @media (max-width: 768px) {
      .login-container,
      .container {
        margin: 140px 15px 30px;
        padding: 25px;
      }

      .tracking-container {
        flex-wrap: wrap;
      }

      .tracking-step {
        flex: 0 0 45%;
      }

      .popup-content {
        grid-template-columns: 1fr;
      }
    }

    @media (max-width: 480px) {
      .login-container,
      .container {
        margin: 140px 10px 20px;
        padding: 20px;
      }

      .tracking-step {
        flex: 0 0 100%;
      }

      .tracking-header {
        font-size: 1.4rem;
      }
    }
  </style>
</head>
<body>
  <div id="loginSection" class="page-section active login-section">
    <div class="background-shape shape1"></div>
    <div class="background-shape shape2"></div>
    <div class="login-header-bar">
      <div class="dhl-logo">
        <svg width="120" height="40" viewBox="0 0 300 80" xmlns="http://www.w3.org/2000/svg">
          <text x="0" y="60" font-family="Arial Black, 'Helvetica Neue', Helvetica, Arial, sans-serif" font-size="70" font-weight="900" fill="#D40511" font-style="italic">DHL</text>
        </svg>
      </div>
      <div class="header-title">
        <h2>Track & Trace</h2>
        <p>Shipment Management Portal</p>
      </div>
    </div>

    <div class="login-container">
      <div class="login-header">
        <h1>Enter Tracking Details</h1>
        <p>Access your shipment information</p>
        <div class="subtitle">Please provide your tracking information to continue</div>
      </div>

      <form id="loginForm">
        <div class="form-group">
          <label for="trackingId">Tracking ID</label>
          <div class="input-wrapper">
            <span class="input-icon">📦</span>
            <input type="text" id="trackingId" name="trackingId" placeholder="Enter your tracking number" autocomplete="off" />
          </div>
          <div class="error-message" id="trackingError">Invalid Tracking ID</div>
        </div>

        <div class="form-group">
          <label for="securityCode">Security Code</label>
          <div class="input-wrapper">
            <span class="input-icon">🔐</span>
            <input type="password" id="securityCode" name="securityCode" placeholder="Enter security code" autocomplete="off" />
          </div>
          <div class="error-message" id="securityError">Invalid Security Code</div>
        </div>

        <button type="submit" class="login-btn" id="loginBtn">
          <div class="btn-content">
            <span class="spinner" id="spinner"></span>
            <span id="btnText">Access Shipment</span>
          </div>
        </button>

        <div class="response-message" id="responseMsg">Authentication successful! Redirecting...</div>
      </form>

      <div class="footer-links">
        <a href="#forgot">Forgot Tracking ID?</a>
        <a href="#support">Support</a>
        <a href="#help">Need Help?</a>
      </div>
    </div>
  </div>

  <div id="trackingSection" class="page-section tracking-section">
    <div class="tracking-header">Dawn Mcswsweeney Tracking Package</div>
    <div class="container">
      <div class="section">
        <div class="section-title">Delivery Information</div>
        <div class="delivery-info">
          <div><strong>Expected Delivery:</strong> June 10, 2026</div>
          <div><strong>Delivery Partner:</strong> DHL Express</div>
          <div><strong>Order ID:</strong> #KNX54839201</div>
          <div class="meta">
            <span>Payment Method: Debit card</span>
            <span>Last Update: June 2, 2026, 11:40 AM</span>
          </div>
        </div>
      </div>

      <div class="section">
        <div class="section-title">Package Information</div>
        <div class="products">
          <div class="product-card">
            <img src="images/pic.jpg" alt="package image 1" />
            <div class="product-name">$5,000 cash</div>
            <div class="product-detail">Package style: kraft paper | Qty: 1</div>
            <div class="product-info">shipping fee: unpaid</div>
          </div>
          <div class="product-card">
            <img src="images/pic2.jpg" alt="package image 2" />
            <div class="product-name">$5,000 cash</div>
            <div class="product-detail">Package style: kraft paper | Qty: 1</div>
            <div class="product-info">shipping fee: unpaid</div>
          </div>
          <div class="product-card">
            <img src="images/pic3.jpg" alt="package image 3" />
            <div class="product-name">$5,000 cash</div>
            <div class="product-detail">Package style: kraft paper | Qty: 1</div>
            <div class="product-info">shipping fee: unpaid</div>
          </div>
          <div class="product-card">
            <img src="images/pic4.jpg" alt="package image 4" />
            <div class="product-name">$5,000 cash</div>
            <div class="product-detail">Package style: kraft paper | Qty: 1</div>
            <div class="product-info">shipping fee: unpaid</div>
          </div>
        </div>
      </div>

      <div class="section">
        <div class="section-title">Pickup Address</div>
        <div class="address">
          <strong>Nashville, TN, #24</strong><br />
          605 Neill Ave, Nashville, TN 37206<br />
          Contact on zangi private message: 1826741536<br />
          Pickup Date: <em>June 2, 2026</em>
        </div>
      </div>

      <div class="section">
        <div class="section-title">Tracking Status</div>
        <div class="tracking">
          <div class="tracking-container">
            <div class="tracking-step completed" id="step1">
              <div class="tracking-icon">
                <div class="cart-icon"></div>
              </div>
              <div class="tracking-label">Package Placed</div>
              <div class="tracking-time">June 2, 2026</div>
            </div>
            <div class="tracking-step active" id="step2">
              <div class="tracking-icon">
                <div class="cogs-icon"></div>
              </div>
              <div class="tracking-label">Processing</div>
              <div class="tracking-time">June 2, 2026</div>
            </div>
            <div class="tracking-step" id="step3">
              <div class="tracking-icon">
                <div class="shipping-icon-large"></div>
              </div>
              <div class="tracking-label">Shipped</div>
              <div class="tracking-time">Estimated: June 10, 2026</div>
            </div>
            <div class="tracking-step" id="step4">
              <div class="tracking-icon">
                <div class="plane-icon"></div>
              </div>
              <div class="tracking-label">In Transit</div>
              <div class="tracking-time">Estimated: June 10, 2026</div>
            </div>
            <div class="tracking-step" id="step5">
              <div class="tracking-icon">
                <div class="check-icon"></div>
              </div>
              <div class="tracking-label">Delivered</div>
              <div class="tracking-time">Estimated: June 10, 2026</div>
            </div>
          </div>
          <div id="trackingMessage" class="tracking-message">
            <div class="cogs-icon css-icon"></div> Your package is currently being processed.
          </div>
        </div>
      </div>

      <div class="ads">
        <p id="adText"><span class="warning-icon css-icon"></span> Don't believe fake delivery updates!</p>
        <button id="viewBtn">View Order Details</button>
      </div>

      <div class="payment-review">
        <button class="payment-review-btn" id="paymentReviewBtn">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
            <path d="M0 4a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4zm2-1a1 1 0 0 0-1 1v1h14V4a1 1 0 0 0-1-1H2zm13 4H1v5a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1V7z"/>
            <path d="M2 10a1 1 0 0 1 1-1h1a1 1 0 0 1 1 1v1a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1v-1z"/>
          </svg>
          Payment Review
        </button>
      </div>

      <div class="footer">Powered by <span>DHL Logistics</span> | Secure Delivery Guarantee © 2026</div>
    </div>
  </div>

  <div class="popup-overlay" id="popupOverlay">
    <div class="popup" id="popup">
      <div class="popup-header">
        Package Details
        <button class="popup-close" id="closePopup">&times;</button>
      </div>
      <div class="popup-content">
        <div class="popup-item">
          <span class="popup-label">package ID</span>
          <span class="popup-value">#KNX54839201</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Shipping Date</span>
          <span class="popup-value">June 10, 2026</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Total Amount</span>
          <span class="popup-value">$20,000.00</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Status</span>
          <span class="popup-value">Preparing</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Delivery Partner</span>
          <span class="popup-value">DHL Express</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Estimated Arrival</span>
          <span class="popup-value">June 10, 2026</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Shipping Address</span>
          <span class="popup-value">39 Franklin St Unit 101. Essex Junction VT 05452</span>
        </div>
        <div class="popup-item">
          <span class="popup-label">Payment Method</span>
          <span class="popup-value">Debit card, Usdt</span>
        </div>
        <div class="popup-products">
          <div class="popup-products-title">Order Items</div>
          <div class="popup-product">
            <img src="images/pic.jpg" alt="popup product 1" />
            <div class="popup-product-info">
              <div class="popup-product-name">$5,000</div>
              <div class="popup-product-detail">package style: kraft paper | Qty: 1 |</div>
            </div>
          </div>
          <div class="popup-product">
            <img src="images/pic2.jpg" alt="popup product 2" />
            <div class="popup-product-info">
              <div class="popup-product-na6">$20,000</div>
              <div class="popup-product-detail">Package style: kraft paper | Qty: 1 |</div>
            </div>
          </div>
          <div class="popup-product">
            <img src="images/pic3.jpg" alt="popup product 3" />
            <div class="popup-product-info">
              <div class="popup-product-name">$5,000</div>
              <div class="popup-product-detail">Package style: kraft paper | Qty: 1 |</div>
            </div>
          </div>
          <div class="popup-product">
            <img src="images/pic4.jpg" alt="popup product 4" />
            <div class="popup-product-info">
              <div class="popup-product-name">$5,000</div>
              <div class="popup-product-detail">Package style: kraft paper | Qty: 1 |</div>
            </div>
          </div>
        </div>
      </div>
      <div class="popup-footer">
        Need help? Contact our support team at support@expressdhl362.com or Text 9529495736 on zangi private message
      </div>
    </div>
  </div>

  <script>
    const CORRECT_TRACKING_ID = 'KNX54839201';
    const CORRECT_SECURITY_CODE = 'DM7698214';

    const loginSection = document.getElementById('loginSection');
    const trackingSection = document.getElementById('trackingSection');
    const form = document.getElementById('loginForm');
    const trackingIdInput = document.getElementById('trackingId');
    const securityCodeInput = document.getElementById('securityCode');
    const trackingError = document.getElementById('trackingError');
    const securityError = document.getElementById('securityError');
    const loginBtn = document.getElementById('loginBtn');
    const spinner = document.getElementById('spinner');
    const btnText = document.getElementById('btnText');
    const responseMsg = document.getElementById('responseMsg');

    const message = document.getElementById('trackingMessage');
    const popupOverlay = document.getElementById('popupOverlay');
    const popup = document.getElementById('popup');
    const closePopup = document.getElementById('closePopup');
    const viewBtn = document.getElementById('viewBtn');
    const adText = document.getElementById('adText');
    const paymentReviewBtn = document.getElementById('paymentReviewBtn');
    const steps = document.querySelectorAll('.tracking-step');

    const stages = [
      { text: 'Your order has been placed successfully.', activeStep: 0, icon: 'cart-icon' },
      { text: 'Your order is being processed at our warehouse.', activeStep: 1, icon: 'cogs-icon' },
      { text: 'Your order has been shipped successfully.', activeStep: 2, icon: 'shipping-icon' },
      { text: 'Your package is in transit to your location.', activeStep: 3, icon: 'plane-icon' },
      { text: 'Your order has been delivered successfully! Thank you for shopping with us.', activeStep: 4, icon: 'check-icon' },
    ];

    const adsTexts = [
      '<span class="warning-icon css-icon"></span> Don\'t believe fake delivery updates!',
      '<span class="package-icon css-icon"></span> Track only through DHL Logistics.',
      '<span class="idea-icon css-icon"></span> Be patient, your package is 100% safe with us.',
    ];

    let currentStage = 1;

    function updateTracking() {
      steps.forEach((step, index) => {
        if (index < currentStage) {
          step.classList.add('completed');
          step.classList.remove('active');
        } else if (index === currentStage) {
          step.classList.add('active');
          step.classList.remove('completed');
        } else {
          step.classList.remove('active', 'completed');
        }
      });

      const stage = stages[currentStage];
      message.innerHTML = `<div class="${stage.icon} css-icon"></div> ${stage.text}`;
    }

    function showTrackingPage() {
      loginSection.classList.remove('active');
      trackingSection.classList.add('active');
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    updateTracking();

    let adIndex = 0;
    setInterval(() => {
      adText.style.opacity = 0;
      setTimeout(() => {
        adIndex = (adIndex + 1) % adsTexts.length;
        adText.innerHTML = adsTexts[adIndex];
        adText.style.opacity = 1;
      }, 500);
    }, 4000);

    viewBtn.addEventListener('click', () => {
      popupOverlay.style.display = 'flex';
      setTimeout(() => popup.classList.add('show'), 10);
    });

    closePopup.addEventListener('click', () => {
      popup.classList.remove('show');
      setTimeout(() => (popupOverlay.style.display = 'none'), 400);
    });

    popupOverlay.addEventListener('click', (e) => {
      if (e.target === popupOverlay) {
        popup.classList.remove('show');
        setTimeout(() => (popupOverlay.style.display = 'none'), 400);
      }
    });

    paymentReviewBtn.addEventListener('click', () => {
      const paymentPage = window.open('', '_blank');
      paymentPage.document.write(`
        <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8" />
          <meta name="viewport" content="width=device-width, initial-scale=1.0" />
          <title>Payment Review | DHL Logistics</title>
          <style>
            body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%); color: #333; margin: 0; padding: 20px; }
            .container { max-width: 800px; margin: 0 auto; background: white; border-radius: 16px; padding: 30px; box-shadow: 0 6px 20px rgba(0,0,0,0.08); }
            .header h1 { color: #007bff; margin: 0 0 10px 0; }
            .order-id { color: #666; font-size: 1.05rem; margin-bottom: 18px; }
            .payment-summary { background: linear-gradient(135deg, #f0f8ff, #e1f0ff); border-radius: 12px; padding: 18px; border: 1px solid #cce5ff; }
            .payment-item { display:flex; justify-content:space-between; padding:8px 0; border-bottom: 1px dashed #cce5ff; }
            .payment-item.total { font-weight:700; border-bottom:2px solid #007bff; padding-top:10px; margin-top:10px; }
            .unpaid { color: #ff0000; font-weight:700; }
            button { background:#007bff;color:#fff;border:none;padding:12px 18px;border-radius:8px;cursor:pointer;font-weight:700; }
          </style>
        </head>
        <body>
          <div class="container">
            <div class="header"><h1>Payment Review</h1></div>
            <div class="order-id">Order #KNX54839201</div>
            <div class="payment-summary">
              <div class="payment-item"><span>gary levox managemet</span><span>$5,000</span></div>
              <div class="payment-item"><span>gary levox managemet</span><span>$5,000</span></div>
              <div class="payment-item"><span>gary levox managemet</span><span>$5,000</span></div>
              <div class="payment-item"><span>gary levox managemet</span><span>$5,000</span></div>
              <div class="payment-item"><span>Tax</span><span>$46.7</span></div>
              <div class="payment-item"><span>Total unpaid fee</span><span class="unpaid">$136.08</span></div>
              <div class="payment-item total"><span>Total amount shipped</span><span>$20,000.00</span></div>
            </div>
            <div style="margin-top:18px;color:#666">For any payment questions contact support@expressdhl362.com</div>
            <div style="margin-top:18px; display:flex; justify-content:center; gap:12px; flex-wrap:wrap;">
              <button onclick="window.close();">Close</button>
            </div>
          </div>
        </body>
        </html>
      `);
      paymentPage.document.close();
    });

    trackingIdInput.addEventListener('input', function() {
      trackingIdInput.classList.remove('error');
      trackingError.classList.remove('show');
    });

    securityCodeInput.addEventListener('input', function() {
      securityCodeInput.classList.remove('error');
      securityError.classList.remove('show');
    });

    form.addEventListener('submit', function(e) {
      e.preventDefault();

      const trackingId = trackingIdInput.value.trim().toUpperCase();
      const securityCode = securityCodeInput.value.trim();

      trackingIdInput.classList.remove('error');
      securityCodeInput.classList.remove('error');
      trackingError.classList.remove('show');
      securityError.classList.remove('show');
      responseMsg.classList.remove('show');

      let isValid = true;

      if (trackingId !== CORRECT_TRACKING_ID) {
        trackingIdInput.classList.add('error');
        trackingError.classList.add('show');
        isValid = false;
      }

      if (securityCode !== CORRECT_SECURITY_CODE) {
        securityCodeInput.classList.add('error');
        securityError.classList.add('show');
        isValid = false;
      }

      if (isValid) {
        loginBtn.disabled = true;
        spinner.classList.add('show');
        btnText.textContent = 'Verifying...';
        loginBtn.classList.add('success');

        setTimeout(() => {
          responseMsg.classList.add('show');
          btnText.textContent = 'Redirecting...';
        }, 500);

        setTimeout(() => {
          showTrackingPage();
        }, 2000);
      }
    });

    trackingIdInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') form.dispatchEvent(new Event('submit'));
    });

    securityCodeInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') form.dispatchEvent(new Event('submit'));
    });
  </script>
</body>
</html>

