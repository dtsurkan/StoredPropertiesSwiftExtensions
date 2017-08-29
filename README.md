# StoredPropertiesSwiftExtensions

```swift
//: Playground - noun: a place where people can play

import UIKit
import Foundation

protocol PropertyStoring {
    
    associatedtype T
    
    func getAssociatedObject(_ key: UnsafeRawPointer!, defaultValue: T) -> T
}

extension PropertyStoring {
    func getAssociatedObject(_ key: UnsafeRawPointer!, defaultValue: T) -> T {
        guard let value = objc_getAssociatedObject(self, key) as? T else {
            return defaultValue
        }
        return value
    }
}

protocol ToggleProtocol {
    func toggle()
}

enum ToggleState {
    case on
    case off
}

extension UIButton: ToggleProtocol, PropertyStoring {
    
    typealias T = ToggleState
    
    private struct CustomProperties {
        static var toggleState = ToggleState.off
    }
    
    var toggleState: ToggleState {
        get {
            return getAssociatedObject(&CustomProperties.toggleState, defaultValue: CustomProperties.toggleState)
        }
        set {
            return objc_setAssociatedObject(self, &CustomProperties.toggleState, newValue, .OBJC_ASSOCIATION_RETAIN)
        }
    }
    
    func toggle() {
        toggleState = toggleState == .on ? .off : .on
        
        if toggleState == .on {
            // Shows background for status on
        } else {
            // Shows background for status off
        }
    }
}

let a = UIButton()
print(a.toggleState)
a.toggleState = .on
print(a.toggleState)

```
